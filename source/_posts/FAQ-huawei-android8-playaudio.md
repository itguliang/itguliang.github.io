---
title: 华为Android8无法播放.aac文件
date: 2018-06-10
categories:
  - IT
tags:
  - FAQ
abbrlink: 27fd0447
---

修改 `cordova-plugin-media` 源码 `AudioPlayer.java`

#### 1.获取写权限

``` java
public void startPlaying(String file) {

    // 获取写权限(如果file插件升级了5.0.0这句可以不加)
    this.handler.getWritePermission(0);  

    if (this.readyPlayer(file) && this.player != null) {
        this.player.start();
        this.setState(STATE.MEDIA_RUNNING);
        this.seekOnPrepared = 0; //insures this is always reset
    } else {
        this.prepareOnly = false;
    }
}
```

#### 2.下载acc文件

``` java
private void loadAudioFile(String file) throws IllegalArgumentException, SecurityException, IllegalStateException, IOException {
    if (this.isStreaming(file)) {

        try {
            if (file.indexOf(".aac") > -1 && (Build.VERSION_CODES.O <= Build.VERSION.SDK_INT)) {
                File mediaFile = this.DownloadAudioStream(file);

                if (mediaFile != null ) {
                    FileInputStream fis = new FileInputStream(mediaFile);
                    this.player.setDataSource(fis.getFD());
                    fis.close();
                }
                this.player.setAudioStreamType(AudioManager.STREAM_MUSIC);
                this.setState(STATE.MEDIA_STARTING);
                this.player.setOnPreparedListener(this);
                this.player.prepare();
                this.duration = getDurationInSeconds();
            } else {
                this.player.setDataSource(this.handler.cordova.getActivity(), Uri.parse(file));
                this.player.setAudioStreamType(AudioManager.STREAM_MUSIC);
                this.setState(STATE.MEDIA_STARTING);
                this.player.setOnPreparedListener(this);
                this.player.prepareAsync();
            }
        } catch (Exception e) {
            LOG.d(LOG_TAG, "loadAudioFile exception ： " + e.toString());
        }

    }
    else {
        if (file.startsWith("/android_asset/")) {
            String f = file.substring(15);
            android.content.res.AssetFileDescriptor fd = this.handler.cordova.getActivity().getAssets().openFd(f);
            this.player.setDataSource(fd.getFileDescriptor(), fd.getStartOffset(), fd.getLength());
        }
        else {
            File fp = new File(file);
            if (fp.exists()) {
                FileInputStream fileInputStream = new FileInputStream(file);
                this.player.setDataSource(fileInputStream.getFD());
                fileInputStream.close();
            }
            else {
                this.player.setDataSource(Environment.getExternalStorageDirectory().getPath() + "/" + file);
            }
        }
        this.setState(STATE.MEDIA_STARTING);
        this.player.setOnPreparedListener(this);
        this.player.prepare();

        // Get duration
        this.duration = getDurationInSeconds();
    }
}

private File DownloadAudioStream(String stream) {
    File mediaFile = null;
    try {
        URLConnection cn = new URL(stream).openConnection();
        InputStream is = cn.getInputStream();

        mediaFile = new File(generateTempFile());
        FileOutputStream fos = new FileOutputStream(mediaFile);
        byte buf[] = new byte[16 * 1024];
        LOG.i("FileOutputStream", "Download");

        // write to file until complete
        do {
            int numread = is.read(buf);
            if (numread <= 0)
                break;
            fos.write(buf, 0, numread);
        } while (true);
        fos.flush();
        fos.close();
        LOG.i("FileOutputStream", "Saved");

        LOG.d(LOG_TAG, "loadAudioFile ： " + stream);
    } catch (Exception e) {

    }

    return mediaFile;
}
```