# Example Input

Commit ID: a1b2c3d4
Author: John Doe
Date: 2024-01-15

Diff:
```diff
diff --git a/src/services/user.js b/src/services/user.js
index 1234567..89abcde 100644
--- a/src/services/user.js
+++ b/src/services/user.js
@@ -45,7 +45,12 @@ class UserService {
   async updateProfile(userId, data) {
     const user = await this.userRepository.findById(userId);
     if (!user) throw new Error('User not found');
-    
+    
+    // Validate email uniqueness if email is being updated
+    if (data.email && data.email !== user.email) {
+      const existing = await this.userRepository.findByEmail(data.email);
+      if (existing) throw new Error('Email already in use');
+    }
+
     return user.update(data);
   }
 }
```
