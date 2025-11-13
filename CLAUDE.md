# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Repository Overview

This is a monorepo containing two iOS grocery list applications built with Swift:
- **Magic Pantry**: Full-featured digital grocery list with ads, in-app purchases, and advanced features
- **Grocery List Pro**: Simpler version focusing on core grocery list functionality

Both apps share similar Firebase authentication and data persistence architecture but differ in feature complexity and monetization strategies.

## Development Setup

### Prerequisites
- Xcode (macOS required)
- CocoaPods installed (`sudo gem install cocoapods`)
- Firebase projects configured (GoogleService-Info.plist files are already in place)

### Initial Setup
```bash
# Install dependencies for Magic Pantry
cd "Magic Pantry"
pod install

# Install dependencies for Grocery List Pro
cd "../Grocery List Pro"
pod install
```

**IMPORTANT**: Always open the `.xcworkspace` file, NOT the `.xcodeproj` file, as this project uses CocoaPods.

### Building and Running
Open the appropriate workspace in Xcode:
- Magic Pantry: `Magic Pantry/Magic Pantry.xcworkspace`
- Grocery List Pro: `Grocery List Pro/Grocery List Pro.xcworkspace`

Build using Xcode's UI (Cmd+B) or xcodebuild:
```bash
# Build Magic Pantry
xcodebuild build -workspace "Magic Pantry/Magic Pantry.xcworkspace" -scheme "Magic Pantry" -destination "platform=iOS Simulator,name=iPhone 15"

# Build Grocery List Pro
xcodebuild build -workspace "Grocery List Pro/Grocery List Pro.xcworkspace" -scheme "Grocery List Pro" -destination "platform=iOS Simulator,name=iPhone 15"
```

### Running Tests
```bash
# Build for testing
xcodebuild build-for-testing -workspace "Magic Pantry/Magic Pantry.xcworkspace" -scheme "Magic Pantry" -destination "platform=iOS Simulator,name=iPhone 15"

# Run tests
xcodebuild test-without-building -workspace "Magic Pantry/Magic Pantry.xcworkspace" -scheme "Magic Pantry" -destination "platform=iOS Simulator,name=iPhone 15"
```

## Architecture

### Magic Pantry
The main application with comprehensive features:

**Core Controllers:**
- `ViewController.swift`: Main entry point, handles Firebase authentication with FirebaseUI, manages user session, integrates Qonversion for IAP
- `Lists.swift` / `Lists(With Ads).swift`: List management screens with ad/no-ad variants
- `SubLists.swift` / `subLists(With Ads).swift`: Detail view for individual lists with ad/no-ad variants
- `NewOnboarding.swift` / `onboarding.swift`: User onboarding flows using paper-onboarding and BulletinBoard pods

**Services:**
- `IAPManager.swift`: In-app purchase management using Qonversion
- `IAP_AppleManager.swift`: Apple StoreKit integration
- `FirebaseUI.swift`: Firebase authentication UI configuration
- `AppiconService.swift`: Alternate app icon management
- `functions.swift`: Shared utility functions
- `ExtraCode.swift`: Additional helper code

**Key Dependencies (Podfile):**
- Firebase ecosystem (Auth, Analytics, Database, Storage)
- FirebaseUI (with Google and Phone auth providers)
- Google-Mobile-Ads-SDK for monetization
- paper-onboarding and BulletinBoard for UX
- Qonversion for IAP management

**Monetization Strategy:**
- Dual view controller approach: files with "(With Ads)" suffix show ads, premium users use ad-free versions
- In-app purchases managed through Qonversion
- Google Mobile Ads integration

### Grocery List Pro
Simplified version with cleaner architecture:

**Structure:**
- `TableViewController.swift`: Main list interface
- `Log-In Page.swift`: Authentication screen
- `UserSettings.swift`: User preferences
- `Store_Items.swift`: Store item management

**Data Models (`Model/`):**
- `Grocery List.swift`: List entity
- `Grocery Store.swift`: Store entity
- `User.swift`: User entity
- `extraUserInfo.swift`: Additional user metadata

**Key Dependencies (Podfile):**
- Firebase (Core, Auth)
- FirebaseUI (Auth, Google, Phone providers)

### Firebase Integration
Both apps use Firebase Realtime Database for data persistence with the following architecture:
- User authentication via FirebaseUI supporting multiple providers (Email, Google, Phone)
- Real-time data synchronization for grocery lists
- User-scoped data access patterns
- GoogleService-Info.plist configuration files located in each app's root directory

### Assets and Resources
- `Fonts/`: Custom font files (Girassol, Poppins, Quicksand)
- `Icons_And_Images/`: App icons and UI images
- `Logo/`: Magic Pantry branding assets
- `Alternate Icons/`: Alternative app icons for Magic Pantry (in-app icon switching)

## CI/CD

GitHub Actions workflow (`.github/workflows/ios.yml`) automatically builds and tests on push/PR to master:
- Runs on macOS-latest
- Automatically detects default scheme
- Builds for iOS Simulator
- Runs unit and UI tests

The workflow uses the `.xcworkspace` files and runs `build-for-testing` followed by `test-without-building`.

## Important Development Notes

### CocoaPods Management
After modifying Podfile, always run:
```bash
cd "Magic Pantry"  # or "Grocery List Pro"
pod install
pod update  # if needed to update pod versions
```

### Firebase Configuration
- Each app has its own `GoogleService-Info.plist` in its root directory
- Do not commit changes to these files with sensitive data
- When working with Firebase features, ensure you're testing with the correct project

### Ad Integration (Magic Pantry only)
When modifying list or sublist features:
- Consider both ad and non-ad versions of view controllers
- Ad-supported versions use Google Mobile Ads SDK
- Premium users (via IAP) see ad-free versions

### Deployment Target
Both apps target iOS 13.0 as minimum deployment target (set in Podfiles).

## Code Style Conventions
- Swift naming conventions (PascalCase for types, camelCase for variables/functions)
- File names with spaces and parentheses (e.g., "Lists(With Ads).swift")
- Firebase delegates and protocols implemented in view controllers
- Storyboard-based UI (`Main.storyboard` in Magic Pantry, programmatic UI in some Grocery List Pro views)
