# Repository Guidelines

## Project Structure & Module Organization
Magic Pantry SuperApp is a mono-repo housing two Swift iOS targets: `Magic Pantry/` (premium + ads toggled via paired view controllers) and `Grocery List Pro/` (lightweight list manager). Each module includes its own `*.xcworkspace`, `*.xcodeproj`, `Tests`/`UITests` bundles, `Podfile`, and `Pods/`. Shared branding assets live at the repo root (`Fonts/`, `Icons_And_Images/`, `Logo/`), while `.github/workflows/ios.yml` defines CI. Always keep pods and assets scoped inside their respective app folders to avoid cross-app leakage.

## Build, Test, and Development Commands
- `cd "Magic Pantry" && pod install`: sync CocoaPods any time the Podfile changes; repeat for `Grocery List Pro`.
- `xcodebuild build -workspace "Magic Pantry/Magic Pantry.xcworkspace" -scheme "Magic Pantry" -destination "platform=iOS Simulator,name=iPhone 15"`: deterministic simulator build; swap scheme/workspace for Grocery List Pro.
- `xcodebuild test-without-building ...`: executes `Magic PantryTests` and `Magic PantryUITests`; mirrors CI’s `build-for-testing` + `test-without-building` split.
- `npm test` / `npm lint`: run before commits if any JS tooling or scripts were touched (per repo policy), even though Swift is primary.

## Coding Style & Naming Conventions
Use Swift 5 defaults: 4-space indentation, PascalCase types, camelCase members, and explicit access control. Magic Pantry keeps file names with spaces or `(With Ads)` suffixes to pair premium/ad variants—preserve that scheme. Follow Cocoa MVC patterns already present (UIViewController subclasses for screens, service singletons like `IAPManager`). Format via Xcode’s “Editor > Structure > Re-Indent”; run `swiftlint` if the pod is pulled in locally.

## Testing Guidelines
Unit/UI tests live under `Magic PantryTests`, `Magic PantryUITests`, and their Grocery List Pro counterparts. Name test files `<Feature>Tests.swift` and test methods `test_<Scenario>_<Expectation>()`. Target ≥80% coverage on new Swift files and add smoke UI tests when modifying onboarding, purchase, or list flows. Prior to pushing, run both the standard simulator test command and any scenario-specific scripts (e.g., Firebase emulators) you introduced. Re-run workflows locally if you modify `.github/workflows/ios.yml`.

## Commit & Pull Request Guidelines
History favors short, imperative messages (`Add iOS starter workflow`, `[FIXED] App Store Release 3.7.0`). Stick to 72-character subject lines, optionally prefixing tags in brackets for release or bugfix context. Each PR should include: goal summary, testing evidence (command output or screenshots for UI work), linked issues/Trello cards, screenshots or videos for UI-affecting changes, and explicit callouts for schema/migration updates. Require reviewers for both apps when shared assets or pods change, and wait for CI green before merging.
