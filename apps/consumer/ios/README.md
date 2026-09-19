# apps/consumer/ios — Native Consumer iOS App

## Status

🔮 **Planned — Implementation not yet initialized.**

## Technology Stack

- **Language**: Swift
- **UI Framework**: SwiftUI
- **Architecture**: The Composable Architecture (TCA) / MVVM with Clean Architecture
- **Build System**: Xcode / Swift Package Manager (SPM)
- **Backend SDK**: `supabase-swift` (PostgREST, Auth, Realtime, Storage)
- **Audio/Speech**: Speech framework / AVFoundation
- **Local Storage**: SwiftData / CoreData / Keychain

## Core Responsibilities

- Native iOS customer experience adhering to Apple Human Interface Guidelines (HIG).
- SiriKit / voice capture integration for the conversational AI Concierge.
- Push notifications via Apple Push Notification service (APNs).
- Secure payment integration with mobile wallet flows (eSewa, Khalti, Fonepay, Apple Pay / Card).
- MapKit and CoreLocation integration for live order and ride tracking.
- Secure token storage in iOS Keychain with Face ID / Touch ID protection.

## Reference Implementation

Refer to the working web prototype at [`prototype/Phone/`](../../../prototype/Phone/) for feature workflows, state machines, and UX specifications.

> **Security Reminder**: Never place Supabase `service_role` keys, private gateway secrets, or API credentials into Xcode build configurations, `Info.plist`, or app bundles.
