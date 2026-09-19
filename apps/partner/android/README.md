# apps/partner/android — Native Partner Android App

## Status

🔮 **Planned — Implementation not yet initialized.**

## Technology Stack

- **Language**: Kotlin
- **UI Framework**: Jetpack Compose
- **Architecture**: MVI / Clean Architecture
- **Build System**: Gradle (Kotlin DSL)
- **Backend SDK**: `supabase-kt` (Auth, PostgREST, Realtime, Storage)
- **Audio/Alerts**: Android MediaPlayer / RingtoneManager for persistent high-priority order alarms
- **Printing**: ESC/POS thermal receipt printer integration (Bluetooth / USB / LAN)

## Core Responsibilities

- Dedicated merchant, restaurant, and rider mobile application.
- High-reliability incoming order reception with wake-lock and foreground service alerts.
- Quick order acknowledgment, prep-time adjustments, and kitchen handoff.
- POS terminal capabilities and order receipt printing for physical store counters.
- Dedicated driver mode for mobility and delivery partners with turn-by-turn map navigation.

> **Security Reminder**: Never store Supabase `service_role` keys, private gateway secrets, or banking credentials inside Android application bundles.
