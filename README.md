# lindroid roms for Redmi K60/POCO F5 Pro
# changelog
# 2024/09/07
## Upload EvolutionX
# 2024/08-29
## Upload Matrixx
# 2024/08-30
## Upload GKI without KernelSu  
## Upload RisingOS
Note that starting from this RisingOS build, SELinux and hw overlay are disabled with some HACK to make lindroid work by default.
#### 0001-disable-selinux.patch ####
```
From 83b2b607e190522109adca0a71ce5a7ec48a5b2e Mon Sep 17 00:00:00 2001
From: HeCheng Yu <kde-yyds@qq.com>
Date: Fri, 30 Aug 2024 17:28:05 +0800
Subject: [PATCH] disable selinux

---
 init/selinux.cpp | 4 ++--
 1 file changed, 2 insertions(+), 2 deletions(-)
 mode change 100644 => 100755 init/selinux.cpp

diff --git a/init/selinux.cpp b/init/selinux.cpp
old mode 100644
new mode 100755
index 1f211dd..c68cedf
--- a/init/selinux.cpp
+++ b/init/selinux.cpp
@@ -105,10 +105,10 @@ EnforcingStatus StatusFromProperty() {
 }
 
 bool IsEnforcing() {
+    return false;
     if (ALLOW_PERMISSIVE_SELINUX) {
         return StatusFromProperty() == SELINUX_ENFORCING;
     }
-    return true;
 }
 
 // Forks, executes the provided program in the child, and waits for the completion in the parent.
@@ -499,7 +499,7 @@ void SelinuxSetEnforcement() {
     bool is_enforcing = IsEnforcing();
     if (kernel_enforcing != is_enforcing) {
         if (security_setenforce(is_enforcing)) {
-            PLOG(FATAL) << "security_setenforce(" << (is_enforcing ? "true" : "false")
+            PLOG(INFO) << "security_setenforce(" << (is_enforcing ? "true" : "false")
                         << ") failed";
         }
     }
--
2.46.0


```


#### 0001-revert-refresh-rate-6-commit-and-disable-hw-overlay.patch
```
From 58607c80e890b5ce8b1fd868ecfece9a0cb9b1c7 Mon Sep 17 00:00:00 2001
From: HeCheng Yu <kde-yyds@qq.com>
Date: Fri, 30 Aug 2024 20:05:29 +0800
Subject: [PATCH] revert refresh rate -6 commit and disable hw overlay when
 starting perspectived service

---
 app/app/src/main/java/org/lindroid/ui/DisplayActivity.java | 2 +-
 perspectived/PerspectiveService.cpp                        | 1 +
 2 files changed, 2 insertions(+), 1 deletion(-)
 mode change 100644 => 100755 app/app/src/main/java/org/lindroid/ui/DisplayActivity.java
 mode change 100644 => 100755 perspectived/PerspectiveService.cpp

diff --git a/app/app/src/main/java/org/lindroid/ui/DisplayActivity.java b/app/app/src/main/java/org/lindroid/ui/DisplayActivity.java
old mode 100644
new mode 100755
index fdc7769..fcc7f9e
--- a/app/app/src/main/java/org/lindroid/ui/DisplayActivity.java
+++ b/app/app/src/main/java/org/lindroid/ui/DisplayActivity.java
@@ -226,7 +226,7 @@ public class DisplayActivity extends AppCompatActivity implements SurfaceHolder.
             } catch (Exception e) {
                 Log.e(TAG, "Failed to get display refresh rate", e);
             }
-            nativeSurfaceChanged(mDisplayID, surface, getResources().getConfiguration().densityDpi, refresh - 6);
+            nativeSurfaceChanged(mDisplayID, surface, getResources().getConfiguration().densityDpi, refresh);
             if (mPreviousWidth != w || mPreviousHeight != h) {
                 nativeReconfigureInputDevice(mDisplayID, w, h);
                 mPreviousWidth = w;
diff --git a/perspectived/PerspectiveService.cpp b/perspectived/PerspectiveService.cpp
old mode 100644
new mode 100755
index 73362b9..0e73649
--- a/perspectived/PerspectiveService.cpp
+++ b/perspectived/PerspectiveService.cpp
@@ -32,6 +32,7 @@ using aidl::vendor::lindroid::perspective::LXCContainerManager;
 
 int main(void) {
     umask(0000);
+    system("/system/bin/service call SurfaceFlinger 1008 i32 1");
     auto perspective = ndk::SharedRefBase::make<LXCContainerManager>();
 
     binder_status_t status = AServiceManager_addService(perspective->asBinder().get(), SERVICE_NAME);
-- 
2.46.0


```
