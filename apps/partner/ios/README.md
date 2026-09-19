# apps/partner/ios — Native Partner iOS App

## Status

🔮 **Planned — Implementation not yet initialized.**

## Technology Stack

- **Language**: Swift
- **UI Framework**: SwiftUI
- **Architecture**: The Composable Architecture (TCA) / MVVM
- **Build System**: Xcode / Swift Package Manager (SPM)
- **Backend SDK**: `supabase-swift` (Auth, PostgREST, Realtime, Storage)
- **Background Tasks**: BackgroundTasks framework + critical alerts for immediate order dispatch
- **Printing**: AirPrint and CoreBluetooth ESC/POS thermal receipt printing

## Core Responsibilities

- Dedicated iPad/iPhone merchant and service provider application.
- Real-time kitchen display system (KDS) and counter order management.
- Live order audio chimes and badge updates via APNs Critical Alerts.
- Table-side ordering and POS receipt printing for restaurant and retail partners.
- Driver trip acceptance and live dispatch view for delivery/ride partners.

> **Security Reminder**: Never place Supabase `service_role` keys, private gateway secrets, or banking credentials inside iOS app bundles or build configurations.
