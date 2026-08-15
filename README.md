[![GitHub Pages](https://img.shields.io/badge/GitHub%20Pages-live-2ea44f?logo=github)](https://dimillian.github.io/Skills/)

# Skills Public

A collection of specialized skills for iOS and Swift development workflows.

## Overview

This repository contains a set of focused skills designed to assist with common iOS development tasks, from generating release notes to debugging apps and maintaining code quality.

Install: place these skill folders under `$CODEX_HOME/skills/public` (or symlink this repo there).

Optional: enable the pre-commit hook to keep `docs/skills.json` in sync:
`git config core.hooksPath scripts/git-hooks`

## Skills

### 📝 App Store Changelog

**Purpose**: Generate user-facing App Store release notes from git history.

Automatically collects commits and changes since the last git tag (or a specified ref) and transforms them into clear, benefit-focused release notes suitable for the App Store. Filters out internal-only changes and groups user-visible improvements by theme.

**Key Features**:
- Collects commits and touched files since the last tag
- Identifies user-visible changes vs internal work
- Generates concise, benefit-focused bullet points
- Validates changes map back to actual commits

**Use When**: You need to create App Store "What's New" text or release notes based on git history.

---

### 🐛 iOS Debugger Agent

**Purpose**: Build, run, and debug iOS projects on simulators using XcodeBuildMCP.

Provides a comprehensive workflow for building iOS apps, launching them on simulators, interacting with the UI, and capturing logs. Handles simulator discovery, session setup, and runtime debugging.

**Key Features**:
- Discovers and manages booted simulators
- Builds and runs apps on simulators
- Interacts with UI (tap, type, swipe, gestures)
- Captures and analyzes app logs
- Screenshots and UI inspection

**Use When**: You need to run an iOS app, interact with the simulator UI, inspect on-screen state, or diagnose runtime behavior.

---

### 🧭 GH Issue Fix Flow

**Purpose**: Resolve GitHub issues end-to-end using `gh`, local edits, builds/tests, and git push.

Provides a structured flow for reading issues, implementing fixes, validating with XcodeBuildMCP, and shipping changes with a closing commit.

**Key Features**:
- Fetches full issue context with `gh issue view`
- Guides code discovery and focused edits
- Runs targeted builds/tests via XcodeBuildMCP
- Commits with closing message and pushes

**Use When**: You need to take an issue number, inspect it with `gh`, implement a fix, run tests, and push.

---

### 🔧 SwiftUI View Refactor

**Purpose**: Refactor SwiftUI view files for consistent structure and dependency patterns.

Applies standardized ordering, Model-View (MV) patterns, and correct Observation usage to SwiftUI views. Focuses on making views lightweight, composable, and maintainable.

**Key Features**:
- Enforces consistent view ordering (Environment → State → init → body → helpers)
- Promotes MV patterns over view models when possible
- Handles view models safely (non-optional when possible)
- Ensures correct `@Observable` and `@State` usage
- Supports dependency injection via `@Environment`

**Use When**: You need to clean up a SwiftUI view's structure, handle view models safely, or standardize dependency injection and Observation usage.

---

### 🧰 macOS SwiftPM App Packaging (No Xcode)

**Purpose**: Scaffold, build, and package SwiftPM-based macOS apps without an Xcode project.

Bootstraps a minimal SwiftPM macOS app folder, then uses shell scripts to build, assemble the .app bundle, sign, and optionally notarize or generate Sparkle appcasts.

**Key Features**:
- Bootstrap template for a SwiftPM macOS app layout
- Packaging script to assemble a .app bundle without Xcode
- Dev loop script to package and launch the app
- Optional release signing/notarization and appcast tooling

**Use When**: You need a from-scratch macOS app layout and packaging flow without Xcode.

---

## Usage

Each skill is self-contained with its own documentation. Refer to the `SKILL.md` file in each skill's directory for detailed workflows, guidelines, and examples.

## Contributing

Skills are designed to be focused and reusable. When adding new skills, ensure they:
- Have a clear, single purpose
- Include comprehensive documentation
- Follow consistent patterns with existing skills
- Include reference materials when applicable
