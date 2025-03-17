# Notifications-in-iOS
A SwiftUI project demonstrating Firebase Cloud Messaging (FCM) push notifications with APNs integration. Includes Firebase setup, APNs configuration, notification permission handling, and testing via Firebase Console &amp; cURL. Ideal for iOS developers implementing push notifications.
# SwiftUI Push Notifications with Firebase Cloud Messaging (FCM)

## 📌 Overview
This repository demonstrates how to integrate **push notifications** in a **SwiftUI app** using **Firebase Cloud Messaging (FCM)** and **Apple Push Notification Service (APNs)**. It covers:
- 🔥 Setting up Firebase in an iOS app
- 🔑 Configuring APNs authentication
- 📲 Requesting notification permissions
- 🚀 Sending push notifications via Firebase Console & API
- 🛠️ Debugging push notification issues

## 🚀 Prerequisites
- Xcode **14 or later**
- Swift **5.7+**
- A **physical iOS device** (APNs does not work on the Simulator)
- A **Firebase account** ([Sign up here](https://console.firebase.google.com/))
- An **Apple Developer account** ([Enroll here](https://developer.apple.com/))

---

## 📥 Step 1: Create a Firebase Project
1. Go to [Firebase Console](https://console.firebase.google.com/)
2. Click **"Create a project"** → Enter a project name → Click **Continue**
3. Disable or enable Google Analytics (optional) → Click **Create Project**
4. Once created, click **Continue** to go to the dashboard

---

## 📲 Step 2: Register Your iOS App in Firebase
1. In **Firebase Console**, click on **"Add App"** → Choose **iOS**
2. Enter your **iOS Bundle ID** (found in Xcode under **Target > General > Bundle Identifier**)
3. Click **Register App**
4. **Download `GoogleService-Info.plist`** and move it to your Xcode project (`Runner` directory)
5. Click **Next** and finish setup

---

## 🔑 Step 3: Enable Push Notifications in Xcode
1. Open Xcode and go to **Signing & Capabilities**
2. Click **+ Capability** and add:
   - ✅ Push Notifications
   - ✅ Background Modes → Enable **Remote Notifications**

---

## 📡 Step 4: Upload APNs Authentication Key to Firebase
1. Go to [Apple Developer Portal](https://developer.apple.com/account/)
2. Navigate to **Certificates, Identifiers & Profiles** → **Keys**
3. Click **+** → Enable **Apple Push Notifications service (APNs)**
4. Click **Continue** → Name your key → Click **Register**
5. Download the `.p8` file and **keep it safe** (you can’t re-download it)
6. Go back to **Firebase Console** → **Project Settings > Cloud Messaging**
7. Upload the `.p8` file, enter the **Key ID** and **Team ID**, and click **Upload**

---

## 🔗 Step 5: Install Firebase SDK in Xcode
1. Open **Xcode**
2. Go to **File > Add Packages**
3. Enter Firebase package URL:
   ```
   https://github.com/firebase/firebase-ios-sdk
   ```
4. Install **Firebase Messaging**
5. Click **Add Package**

---

## ⚡ Step 6: Configure Firebase in SwiftUI
### **AppDelegate.swift (For Notifications Handling)**
```swift
import UIKit
import Firebase
import FirebaseMessaging

class AppDelegate: NSObject, UIApplicationDelegate {
    func application(_ application: UIApplication, didFinishLaunchingWithOptions launchOptions: [UIApplication.LaunchOptionsKey: Any]? = nil) -> Bool {
        FirebaseApp.configure()
        UNUserNotificationCenter.current().delegate = self
        Messaging.messaging().delegate = self
        requestNotificationPermission()
        application.registerForRemoteNotifications()
        return true
    }

    func requestNotificationPermission() {
        let authOptions: UNAuthorizationOptions = [.alert, .badge, .sound]
        UNUserNotificationCenter.current().requestAuthorization(options: authOptions) { granted, error in
            if let error = error {
                print("Error requesting notification permission: \(error.localizedDescription)")
            }
            print("Notification permission granted: \(granted)")
        }
    }
}
```

### **Main.swift (Connect AppDelegate with SwiftUI)**
```swift
import SwiftUI

@main
struct PushNotificationApp: App {
    @UIApplicationDelegateAdaptor(AppDelegate.self) var appDelegate
    var body: some Scene {
        WindowGroup {
            ContentView()
        }
    }
}
```

### **ContentView.swift (Test Local Notifications)**
```swift
import SwiftUI
import UserNotifications

struct ContentView: View {
    var body: some View {
        VStack {
            Button("Request Permission") {
                requestNotificationPermission()
            }
            .padding()
            .background(Color.blue)
            .foregroundColor(.white)
            .cornerRadius(10)

            Button("Send Test Notification") {
                sendTestNotification()
            }
            .padding()
            .background(Color.green)
            .foregroundColor(.white)
            .cornerRadius(10)
        }
    }
}
```

## 🚑 Troubleshooting
### ❌ **Not Receiving Notifications?**
- Ensure you **run the app on a real device** (APNs doesn't work on Simulator)
- Check **Settings > Notifications** and enable alerts for your app
- Restart your iPhone and Xcode
- Ensure APNs **Key ID & Team ID** are correct in Firebase
- Verify that **Firebase Messaging** is configured properly in `AppDelegate.swift`

---

## 🎉 Conclusion
This guide helps you integrate and test **push notifications** in **SwiftUI** using **Firebase Cloud Messaging (FCM)**. 🚀

Let me know if you need further assistance! 😊

