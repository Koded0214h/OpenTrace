# OpenTrace MVP Development Roadmap

## Vision
Build the world's first universal anti-theft protocol that turns every smartphone into its own tracking device using a dual-layer system (BLE + Cell Tower Triangulation).

## Core Philosophy
- **Single app protection** - One app on your main phone protects that phone
- **Privacy-first** - Rotating PIDs prevent long-term tracking
- **Hardware-free** - No additional devices required
- **Universal** - Works across Android devices

---

## Phase 1: Foundation (Weeks 1-4)
### Backend (Django/Django REST Framework)
- [ ] Project setup with Django + PostgreSQL + PostGIS
- [ ] User authentication system (JWT tokens)
- [ ] Database models:
  - [ ] `User` model with 2FA support
  - [ ] `Device` model (IMEI, device_info, status)
  - [ ] `LocationPing` model (PID, location, timestamp, source)
  - [ ] `LostDeviceReport` model (device, reported_at, status)
- [ ] Core API endpoints:
  - [ ] `POST /api/auth/register/`
  - [ ] `POST /api/auth/login/`
  - [ ] `POST /api/devices/` (register new device)
  - [ ] `POST /api/devices/<id>/report_lost/`
  - [ ] `GET /api/devices/<id>/location/`

### Cryptographic Protocol
- [ ] Implement rotating PID algorithm:
  ```python
  PID = HASH_SHA256(IMEI + CURRENT_TIME_WINDOW + SECURITY_SALT)
  
  Time-window management (τ=15 minutes)

  Secure key storage and rotation system

---
## Phase 2: Mobile Core (Weeks 5-8)
- Native Android App (Kotlin/Java)
- Authentication & Device Setup
    - Login/registration screens
    - Device registration flow (auto-capture IMEI)
    - "Guardian Mode" activation
- BLE Module
    - Broadcast Service: Background service that broadcasts rotating PID when device is marked lost
    - Scanning Service: Background service that scans for other lost devices
    - Battery-optimized scanning cycles (scan 5s, sleep 30s)
    - "Wi-Fi only" relay mode setting
- CTT (Cell Tower Triangulation) Module
    - Cell tower ID collection (TelephonyManager)
    - Background service for periodic tower scanning
    - Wi-Fi detection and opportunistic data transmission

---
## Phase 3: Integration & Dashboard (Weeks 9-12)
- Backend Enhancements
    - Real-time WebSocket support for live location updates
    - Location processing engine:
    - BLE relay location correlation
    - CTT triangulation algorithm
    - Location accuracy scoring
    - Push notification system (FCM integration)
- Mobile App UI/UX
    - Main dashboard map view
    - Device status overview (safe/lost)
    - "Mark as Stolen" panic button with confirmation
    - Location history timeline
    - Settings (battery preferences, Wi-Fi only mode)
- Web Dashboard
    - Secure login
    - "Mark as Stolen" functionality
    - Real-time map view of device location
    - Location history with source indicators (BLE/CTT)

---
## Phase 4: MVP Polish & Launch (Weeks 13-16)
- Testing & Optimization
    - Battery testing: Ensure <1% daily drain for relay mode
    - BLE range testing: Indoor/outdoor effectiveness
    - CTT accuracy testing: Urban vs rural performance
    - Background service reliability: Ensure survival through Doze mode
- Security & Privacy
    - Security audit of PID rotation algorithm
    - Data encryption at rest and in transit
    - Privacy policy and terms of service
    - "Vigilante Recovery Disclaimer" implementation
- Deployment
    - Production server setup (AWS/Azure/DigitalOcean)
    - CI/CD pipeline for mobile and backend
    - APK distribution channel (bypass Play Store initially)
    - Landing page with MVP explanation

---
## Phase 5: Post-MVP Traction (Months 5-6)
- Community Building
    - Open-source core protocol on GitHub
    - Developer documentation
    - Target niche communities:
    - Travel/digital nomad forums
    - College campus groups
    - Android power user communities
- Partnership Preparation
    - Create carrier pitch deck
    - Develop "Silent Ping" API specification
    - Legal framework for carrier agreements
    - Identify target Tier 2/3 carriers for pilot
- Technical Specifications
    - Backend Stack
        - Framework: Django + Django REST Framework
        - Database: PostgreSQL with PostGIS
        - Authentication: JWT tokens
        - Real-time: Django Channels (WebSockets)
        - Push Notifications: Firebase Cloud Messaging (FCM)
        - Deployment: Docker + Cloud provider
    - Mobile Stack
        - Platform: Native Android (Kotlin)
        - BLE: Android BluetoothAdapter API
        - Location: FusedLocationProvider + TelephonyManager
        - Background: WorkManager + Foreground Services
        - Maps: Google Maps SDK
--
## Key Features for MVP Launch
- Single-app protection — Your phone protects itself
- Dual-layer tracking — BLE + CTT working together
- Privacy-first — Rotating PIDs prevent tracking
- Offline capability — BLE works without internet
- Theft recovery dashboard — Web and mobile views
### Success Metrics for MVP
- Technical: <2% battery drain per day in relay mode
- Performance: Location updates within 30 minutes in urban areas
- User: 1,000 active devices in first 3 months
- Network: 70% of devices participating as relay nodes
### Next Steps After MVP
- Use traction to seek seed funding
- Begin carrier partnership discussions
- Expand to iOS (with limited BLE capabilities)
- Develop manufacturer integration strategy