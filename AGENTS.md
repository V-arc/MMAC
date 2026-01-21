# Repository Guidelines

## Project Structure & Module Organization

- `auto-clicker/`: main Swift/SwiftUI app source, grouped by intent (`Views/`, `Services/`, `Models/`, `Observable Objects/`, `Extensions/`).
- `auto-clicker/Localisation/`: translations in `.lproj` folders (for example `en-GB.lproj/Localizable.strings`).
- `auto-clicker/Build Assets/` + `art/`: Xcode asset catalogs and design references (icons, screenshots).
- `fastlane/`, `Gemfile`, `Makefile`: release automation; Fastlane outputs go to `build/` (gitignored except `.gitkeep`).

## Build, Test, and Development Commands

- `make setup`: installs repo githooks (`core.hooksPath=.githooks`) to validate commit messages.
- CI: builds and linting run via GitHub Actions in `.github/workflows/` (use this if you don't have a local macOS environment).
- Local prereqs: macOS + Xcode (this is a macOS app). Install SwiftLint with `brew install swiftlint`.
- Xcode: open `auto-clicker.xcworkspace` (SwiftPM deps) and Run the `auto-clicker` scheme.
- Lint: `swiftlint --strict` (CI runs SwiftLint in strict mode on PRs).
- Fastlane builds (Ruby/Bundler):
  - `bundle install`
  - `make local`: produces a release build in `build/` (for example `<AppName>.app` and a `.zip`)
  - `make beta` / `make prod`: release lanes used for `staging` / `main`

## Coding Style & Naming Conventions

- Follow `.swiftlint.yml` (including custom rules like keeping SwiftUI `@State/@StateObject/@ObservedObject/@Environment/@Default` properties `private`).
- Prefer `final class` unless subclassing is required.
- Naming: types/files `UpperCamelCase`, functions/vars `lowerCamelCase`.

## Testing Guidelines

- There is no dedicated XCTest target currently; validate changes by running the app in Xcode and exercising the affected flows.
- If you add tests, use XCTest and name files `SomethingTests.swift`. Suggested command:
  `xcodebuild test -workspace auto-clicker.xcworkspace -scheme auto-clicker`

## Commit & Pull Request Guidelines

- Use Conventional Commits (enforced by hook): `feat:`, `fix:`, `docs:`, `chore:`, `refactor:`, `perf:`, `test:`, `style:` (optional scope, and `!` for breaking changes, for example `feat(localisation)!: ...`).
- Branch flow: branch off `dev` -> PR into `dev` -> merge to `staging` (beta) -> merge to `main` (production).
- PRs must link an issue and complete `.github/pull_request_template.md`; include testing notes and screenshots for UI changes.
