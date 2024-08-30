# lindroid roms for Redmi K60/POCO F5 Pro
# changelog
# 2024/09/07
## Upload EvolutionX
# 2024/08-29
## Upload Matrixx
# 2024/08-30
## Upload GKI without KernelSu  
## Upload RisingOS
Note that starting from this RisingOS build, SELinux are disabled with some HACK to make lindroid work by default. You still have to enable `disable HW Overlay` manually  
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
