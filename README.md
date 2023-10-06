# Victoria Arduino App Patched

This app communicates with the machine directly using Bluetooth. However, the server is down, so we cannot get pass the launch screen. 

This is the patch to bypass the launch screen, so we can use the main functionality of the app.

## Prerequisite Tools

- Android Studio (https://developer.android.com/studio)
- VSCode (https://code.visualstudio.com/)
- APKLab (https://github.com/APKLab/APKLab)
- APK file (https://apkcombo.com/victoria-arduino-e1/com.hiroia.vaeagleone/download/apk)

## How to patch

**This instruction is based on apk version 1.7.2**

1. Open VSCode and Click `View -> Command Palatte..`
2. Select `APKLab: Open an APK`
![Step 1](/images/1.png)

3. Click `OK`, and wait for another window of VSCode
![Step 2](/images/2.png)

4. On `smali/com/hiroia/vaeagleone/ui/login/LaunchActivity.smali`, put this code after line 536
```diff
    :cond_0
+   const p2, 0x10008000
+
+   new-instance p1, Landroid/content/Intent;
+
+   const-class p4, Lcom/hiroia/vaeagleone/ui/home/MachineListActivity;
+
+   invoke-direct {p1, p0, p4}, Landroid/content/Intent;-><init>(Landroid/content/Context;Ljava/lang/Class;)V
+
+   invoke-virtual {p1, p2}, Landroid/content/Intent;->setFlags(I)Landroid/content/Intent;
+
+   invoke-virtual {p0, p1}, Lcom/hiroia/vaeagleone/ui/login/LaunchActivity;->startActivity(Landroid/content/Intent;)V
+
+   invoke-virtual {p0}, Lcom/hiroia/vaeagleone/ui/login/LaunchActivity;->finish()V
```
5. On `apktool.yml`, rename manifest package, so that it won't replace the original app while installing
```yaml
packageInfo:
  forcedPackageId: '127'
  renameManifestPackage: com.hiroia.vaeagleone.temp
```
6. On `res/values/strings.xml`, change the app name, so we can easily check which one is the patched app
```xml
    <string name="app_name">Temp Victoria Arduino E1 Prima</string>
```
7. On `AndroidManifest.xml`, modify `android:authorities` of providers
```diff
-    <provider android:authorities="com.hiroia.vaeagleone.firebaseinitprovider"
+    <provider android:authorities="com.hiroia.vaeagleone.temp.firebaseinitprovider"

-    <provider android:authorities="com.hiroia.vaeagleone.lifecycle-process"
+    <provider android:authorities="com.hiroia.vaeagleone.temp.lifecycle-process"
```
8. Right click on `apktool.yml` on Explorer panel on VS Code, and select `APKLab: Rebuild the APK`
![Step 6](/images/6.png)

9. Click `OK`
![Step 7](/images/7.png)

10. After build APK successfully, you will find the patched APK at folder `dist`
![Step 8](/images/8.png)