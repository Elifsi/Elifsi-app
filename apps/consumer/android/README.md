# apps/consumer/android — Native Consumer Android App

## Status

🔮 **Planned — Implementation not yet initialized.**

## Technology Stack

- **Language**: Kotlin
- **UI Framework**: Jetpack Compose
- **Architecture**: MVI / MVVM with Clean Architecture
- **Build System**: Gradle (Kotlin DSL)
- **Backend SDK**: `supabase-kt` (PostgREST, Auth, Realtime, Storage)
- **Networking**: Ktor / OkHttp
- **Local Storage**: Room / DataStore

## Core Responsibilities

- Native Android customer experience with Material You / modern mobile design.
- Voice capture & audio streaming integration for the AI Concierge.
- Push notifications via Firebase Cloud Messaging (FCM).
- In-app payment integration with local wallet SDKs (eSewa, Khalti, Fonepay) and tokenized card gateways.
- Background location and map rendering for ride/delivery tracking.
- Biometric authentication (Fingerprint / Face Unlock).

## Reference Implementation

Refer to the working web prototype at [`prototype/Phone/`](../../../prototype/Phone/) for feature workflows, state machines, and UX specifications.

> **Security Reminder**: Never place Supabase `service_role` keys, private gateway secrets, or API credentials into Android build configs or assets.
