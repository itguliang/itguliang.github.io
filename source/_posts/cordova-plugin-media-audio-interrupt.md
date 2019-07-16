---
title: Android IOS 音频中断处理
date: 2019-01-10
categories:
  - IT
abbrlink: 6fffd4ca
---
### 项目中Android遇到的问题 

由于改源码把原来的音频中断改失效了，恢复即可，其他逻辑可以自行修改

cordova-plugin-media src AudioPlayer.java:
```java
public void pausePlaying() {
    // If playing, then pause
    if (this.state == STATE.MEDIA_RUNNING && this.player != null) {
        this.player.pause();
        this.setState(STATE.MEDIA_PAUSED);
    }else {
        LOG.d(LOG_TAG, "AudioPlayer Error: pausePlaying() called during invalid state: " + this.state.ordinal());
        sendErrorStatus(MEDIA_ERR_NONE_ACTIVE);
    }
}
```

cordova-plugin-media src AudioHandler.java:
```java
public void pauseAllLostFocus() {
    for (AudioPlayer audio : this.players.values()) {
        if (audio.getState() == AudioPlayer.STATE.MEDIA_RUNNING.ordinal()) {
            this.pausedForFocus.add(audio);
            audio.pausePlaying();
        }
    }
}

public void resumeAllGainedFocus() {
    for (AudioPlayer audio : this.pausedForFocus) {
        audio.startPlaying(null,0);
    }
    this.pausedForFocus.clear();
}
private OnAudioFocusChangeListener focusChangeListener = new OnAudioFocusChangeListener() {
    public void onAudioFocusChange(int focusChange) {
        LOG.d("AudioFocus", "OnAudioFocusChangeListener:::"+focusChange);
        switch (focusChange) {
        case (AudioManager.AUDIOFOCUS_LOSS_TRANSIENT_CAN_DUCK) :
        case (AudioManager.AUDIOFOCUS_LOSS_TRANSIENT) :
        case (AudioManager.AUDIOFOCUS_LOSS) :
            pauseAllLostFocus();//被中断
            break;
        case (AudioManager.AUDIOFOCUS_GAIN):
            resumeAllGainedFocus();//中断结束
            break;
        default:
            break;
        }
    }
};
public void getAudioFocus() {
    String TAG2 = "AudioHandler.getAudioFocus(): Error : ";

    AudioManager am = (AudioManager) this.cordova.getActivity().getSystemService(Context.AUDIO_SERVICE);
    int result = am.requestAudioFocus(focusChangeListener,
    AudioManager.STREAM_MUSIC,AudioManager.AUDIOFOCUS_GAIN);

    if (result != AudioManager.AUDIOFOCUS_REQUEST_GRANTED) {
        LOG.e(TAG2,result + " instead of " + AudioManager.AUDIOFOCUS_REQUEST_GRANTED);
    }
    LOG.d("AudioFocus", "getAudioFocus:::"+result);
}
```

### 项目中IOS遇到的问题 

* cordova-plugin-media插件音频播放，被打断后播放停止，但是状态没有改变，还是播放状态

    修改CDVSound.m 文件：

    1.注册中断通知 AVAudioSessionInterruptionNotification

    在iOS设备上添加或移除音频输入,输出线路时,会发生线路改变,比如用户插入耳机或断开USB麦克风.当这些事件发生时,音频会根据情况改变输入或输入线路,同时AVAudioSession会发送一个相关变化的通知AVAudioSessionRouteChangeNotification.注册通知的相关代码如下:

    ```objectivec
    // create方法 注册中断通知
    [[NSNotificationCenter defaultCenter] addObserver:self selector:@selector(handleInterruption:) name:AVAudioSessionInterruptionNotification object:[AVAudioSession sharedInstance]];
    ```

    2.处理中断

    根据苹果公司的文档,当用户插入耳机时,隐含的意思是用户不希望外界听到具体的音频了内容,这就意味着当用户断开耳机时,播放的内容可能需要保密,所以我们需要在断开耳机时停止音频播放.

    AVAudioSessionRouteChangeNotification通知的userinfo中会带有通知发送的原因信息及前一个线路的描述.线路变更的原因保存在userinfo的AVAudioSessionRouteChangeReasonKey值中,通过返回值可以推断出不同的事件,对于旧音频设备中断对应的reason为AVAudioSessionRouteChangeReasonOldDeviceUnavailable.但光凭这个reason并不能断定是耳机断开,所以还需要使用通过AVAudioSessionRouteChangePreviousRouteKey获得上一线路的描述信息,注意线路的描述信息整合在一个输入NSArray和一个输出NSArray中,数组中的元素都是AVAudioSessionPortDescription对象.我们需要从线路描述中找到第一个输出接口并判断其是否为耳机接口,如果为耳机,则停止播放.

    ```objectivec
    // 音频被打断音频停止，状态没停，手动触发更改状态为stop
    -(void)handleInterruption:(NSNotification *)notification{
        NSString* mediaId = self.currMediaId;
        int type = [notification.userInfo[AVAudioSessionInterruptionTypeKey] intValue];
        switch (type) {
            case AVAudioSessionInterruptionTypeBegan: // 被打断
                NSLog(@"AVAudioSessionInterruptionTypeBegan");
                [self onStatus:MEDIA_STATE mediaId:mediaId param:@(MEDIA_STOPPED)];
                break;
            case AVAudioSessionInterruptionTypeEnded: // 中断结束
                NSLog(@"AVAudioSessionInterruptionTypeEnded");
                break;
    }
    ```
* cordova-plugin-media插件音频播放，耳机播放拔出耳机音频播放停止，但是状态没有改变，还是播放状态
    修改CDVSound.m 文件：
    1.注册线路改变通知 AVAudioSessionRouteChangeNotification
    ```objectivec
    //create方法 添加或移除音频输入通知
    [[NSNotificationCenter defaultCenter] addObserver:self selector:@selector(handleRouteChange:) name:AVAudioSessionRouteChangeNotification object:[AVAudioSession sharedInstance]];
    ```
    2.耳机拔出处理
    ```objectivec
    // 耳机拔出音频停止，状态没停，手动触发stop
    - (void)handleRouteChange:(NSNotification *)notification{
        NSString* mediaId = self.currMediaId;
        NSDictionary *info = notification.userInfo;
        AVAudioSessionRouteChangeReason reason = [info[AVAudioSessionRouteChangeReasonKey] unsignedIntegerValue];
        if (reason == AVAudioSessionRouteChangeReasonOldDeviceUnavailable) {  //旧音频设备断开
            //获取上一线路描述信息
            AVAudioSessionRouteDescription *previousRoute = info[AVAudioSessionRouteChangePreviousRouteKey];
            //获取上一线路的输出设备类型
            AVAudioSessionPortDescription *previousOutput = previousRoute.outputs[0];
            NSString *portType = previousOutput.portType;
            if ([portType isEqualToString:AVAudioSessionPortHeadphones]) {
                NSLog(@"AVAudioSessionPortHeadphones");
                [self onStatus:MEDIA_STATE mediaId:mediaId param:@(MEDIA_STOPPED)];
            }
        }
    }
    ```
参考：
https://blog.csdn.net/chenchuntong/article/details/8813719

https://blog.csdn.net/adayabetter/article/details/49275071

https://www.jianshu.com/p/5302ca3ab071

https://www.jianshu.com/p/5d8d7b677690

IOS 中断处理  AVAudioSession

https://blog.csdn.net/jeffasd/article/details/54948968

https://www.jianshu.com/p/3e0a399380df
