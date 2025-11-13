# CODEX Project Brief

## TL;DR
Monorepo with two Swift iOS apps (Magic Pantry and Grocery List Pro) that share Firebase auth/storage patterns plus Cocoapods-managed dependencies. Work from each app's `.xcworkspace`, keep pods in sync, and respect CI's `xcodebuild` build/test flow.

## Project Overview
- **Magic Pantry**: Feature-rich list app with ads, IAP unlocks (Qonversion), alternate icon support, and multiple onboarding flows.
- **Grocery List Pro**: Leaner sister app focused on core list creation with Firebase-backed auth/data.
- Shared assets live at the repo root (`Fonts`, `Icons_And_Images`, `Logo`); each app folder is a self-contained Xcode workspace with matching Pods tree.
- CI (`.github/workflows/ios.yml`) auto-detects the default scheme, then runs `xcodebuild build-for-testing` and `test-without-building` on macOS runners.

## Development Setup
1. Install prerequisites: macOS + Xcode 15, Cocoapods (`sudo gem install cocoapods`), Firebase CLI (optional for debugging).
2. Install pods in both apps:
   ```bash
   cd "Magic Pantry" && pod install
   cd "../Grocery List Pro" && pod install
   ```
3. Always open `*.xcworkspace` files; pods break if you stay on the `.xcodeproj`.

## Build & Test Cheatsheet
```bash
xcodebuild build -workspace "Magic Pantry/Magic Pantry.xcworkspace" -scheme "Magic Pantry" -destination "platform=iOS Simulator,name=iPhone 15"
xcodebuild test-without-building -workspace "Magic Pantry/Magic Pantry.xcworkspace" -scheme "Magic Pantry" -destination "platform=iOS Simulator,name=iPhone 15"
```
Apply the same pattern for Grocery List Pro. Local runs should mirror the CI scheme detection to avoid drift.

## File Tree (top-level)
```
.
├── CLAUDE.md
├── CODEX_PROJECT.md
├── Fonts/
├── Grocery List Pro/
├── Icons_And_Images/
├── Logo/
├── Magic Pantry/
├── README.md
├── colours.txt
└── credits.txt
```
Each app directory contains its workspace (`*.xcworkspace`), project (`*.xcodeproj`), source folder, tests, and Pod manifests/outputs. Keep this tree in sync when moving or adding modules.
