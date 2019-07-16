---
title: Cordova Android 端保存cookie 延时
date: 2018-11-30
categories:
  - IT
tags:
  - Cordova
  - FAQ
abbrlink: b0fc5aa
---
项目中使用了持久化登陆，首次使用应用时需要登陆，登陆成功后后端在本地种植 Cookie，下次开启应用时无需再进行登陆，每次请求接口时都会带上 Cookie，后端校验如果 Cookie 失效会返回失效参数，前端需要跳转到登录界面重新登陆。

问题出现在Android端，如果在登陆后立马杀掉进程退出应用，下次再进入应用时仍然会跳转到登陆页面（我们在应用功能启动时会有一个验证操作，校验 Cookie 是否有效，如果失效就会跳转到登录页面），网上查说是 Android 下 Cookie 保存机制的问题。

网上查到了遇到同样的问题(https://www.jianshu.com/p/2d9a8d38c371) ，不同的是我遇到的是cordova的环境,应该解决方法是一样的，测试了一下，杀掉进程没有调用 onStop 保存cookie, debug 提示CordovaActivity paused，调整了下代码如下：
```java
import android.os.Bundle;
import org.apache.cordova.*;
import android.webkit.CookieSyncManager;
import android.os.Build;
import android.webkit.CookieManager;
import android.util.Log;

public class MainActivity extends CordovaActivity
{
    @Override
    public void onCreate(Bundle savedInstanceState)
    {
        super.onCreate(savedInstanceState);
        initCookie();
        // enable Cordova apps to be started in the background
        Bundle extras = getIntent().getExtras();
        if (extras != null && extras.getBoolean("cdvStartInBackground", false)) {
            moveTaskToBack(true);
        }
        // Set by <content src="index.html" /> in config.xml
        loadUrl(launchUrl);
    }
    @Override 
    protected void onPause() { 
        Log.e("AAA", "onPause");
        saveCookie();
        super.onPause(); 
    }
    @Override
    protected void onStop() {
        Log.e("AAA", "onStop");
        saveCookie();
        super.onStop();
    }
    private void initCookie() {
        Log.e("AAA", "initCookie");
        if (Build.VERSION.SDK_INT < 21) CookieSyncManager.createInstance(this);
    }
    private void saveCookie() {
        Log.e("AAA", "saveCookie");
        if (Build.VERSION.SDK_INT >= 21) CookieManager.getInstance().flush();
        else CookieSyncManager.getInstance().sync();
    }
}
```
登录成功跳到首页，马上杀掉进程，调用了 onPause 保存cookie方法，但是还会偶现 重新打开APP跳到登录页

（待更好方法解决）

参考链接：
https://mockingbot.com/posts/287
https://www.jianshu.com/p/2d9a8d38c371
https://stackoverflow.com/questions/29375418/android-webview-doesnt-store-the-cookies