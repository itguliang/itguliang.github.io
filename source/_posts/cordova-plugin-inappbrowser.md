---
title: cordova-plugin-inappbrowser 关闭之前添加弹框提示
date: 2019-01-09
categories:
  - IT
abbrlink: 197343b8
---
Android 实现：
修改插件源码`plugins/cordova-plugin-inappbrowser/src/android/InAppBrowser.java`
```java
//add import
import android.app.AlertDialog;
import android.app.AlertDialog.Builder;
import android.content.DialogInterface;
import android.annotation.SuppressLint;

// * Closes the dialog 修改 closeDialog 方法
public void closeDialog() {
  AlertDialog.Builder dlg = this.createDialog(this.cordova.getActivity());
  dlg.setTitle("提示title");
  dlg.setMessage("提示message?");
  dlg.setCancelable(true);
  dlg.setPositiveButton("确定",new AlertDialog.OnClickListener() {
    public void onClick(DialogInterface dlgInterface, int which) {
      dlgInterface.dismiss();
      // 原来的关闭方法写在这里
      // cordova.getActivity().runOnUiThread(new Runnable() {
      // ...
      // });
    }
  });
  dlg.setNegativeButton("取消", new AlertDialog.OnClickListener() {
    public void onClick(DialogInterface dlgInterface, int which){
      dlgInterface.dismiss();
    }
  });
  dlg.show();
}
// add createDialog
@SuppressLint("NewApi")
private AlertDialog.Builder createDialog(Context context) {
  int currentapiVersion = android.os.Build.VERSION.SDK_INT;
  if (currentapiVersion >= android.os.Build.VERSION_CODES.HONEYCOMB) {
    return new AlertDialog.Builder(context, AlertDialog.THEME_DEVICE_DEFAULT_LIGHT);
  } else {
    return new AlertDialog.Builder(context);
  }
}
```

遇到问题：
3.0.0版本 footercolor:'#333' Android闪退，设置为'#333333'解决

