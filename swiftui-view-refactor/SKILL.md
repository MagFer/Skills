---
name: swiftui-view-refactor
description: Refactor and review SwiftUI view files for consistent structure, dependency injection, and Observation usage. Use when asked to clean up a SwiftUI view's layout/ordering, implement ViewModels per app section, or standardize how dependencies and @Observable ViewModels are initialized and passed.
---

# SwiftUI View Refactor

## Overview
Apply a consistent structure and dependency pattern to SwiftUI views, with a focus on ordering, ViewModels per app section, proper ViewModel initialization, and correct Observation usage.

## Core Guidelines

### 1) View ordering (top → bottom)
- Environment
- `private`/`public` `let`
- `@State` / other stored properties
- computed `var` (non-view)
- `init`
- `body`
- computed view builders / other view helpers
- helper / async functions

### 2) Use ViewModels per app section
- Each major app section or feature should have its own ViewModel to manage state and logic.
- Use `@Observable` for ViewModels with `@State` in the view.
- Initialize ViewModels non-optionally in the view's `init` by passing dependencies.
- Inject services and shared models via `@Environment`; pass them to ViewModels.
- Keep views declarative and focused on UI; delegate business logic to ViewModels.
- For simple, stateless components, direct `@State` in the view is acceptable.

### 3) Split large bodies and view properties
- If `body` grows beyond a screen or has multiple logical sections, split it into smaller subviews.
- Extract large computed view properties (`var header: some View { ... }`) into dedicated `View` types when they carry state or complex branching.
- It's fine to keep related subviews as computed view properties in the same file; extract to a standalone `View` struct only when it structurally makes sense or when reuse is intended.
- Prefer passing small inputs (data, bindings, callbacks) over reusing the entire parent view state.

Example (extracting a section):

```swift
var body: some View {
    VStack(alignment: .leading, spacing: 16) {
        HeaderSection(title: title, isPinned: isPinned)
        DetailsSection(details: details)
        ActionsSection(onSave: onSave, onCancel: onCancel)
    }
}
```

Example (long body → shorter body + computed views in the same file):

```swift
var body: some View {
    List {
        header
        filters
        results
        footer
    }
}

private var header: some View {
    VStack(alignment: .leading, spacing: 6) {
        Text(title).font(.title2)
        Text(subtitle).font(.subheadline)
    }
}

private var filters: some View {
    ScrollView(.horizontal, showsIndicators: false) {
        HStack {
            ForEach(filterOptions, id: \.self) { option in
                FilterChip(option: option, isSelected: option == selectedFilter)
                    .onTapGesture { selectedFilter = option }
            }
        }
    }
}
```

Example (extracting a complex computed view):

```swift
private var header: some View {
    HeaderSection(title: title, subtitle: subtitle, status: status)
}

private struct HeaderSection: View {
    let title: String
    let subtitle: String?
    let status: Status

    var body: some View {
        VStack(alignment: .leading, spacing: 4) {
            Text(title).font(.headline)
            if let subtitle { Text(subtitle).font(.subheadline) }
            StatusBadge(status: status)
        }
    }
}
```

### 3b) Keep a stable view tree (avoid top-level conditional view swapping)
- Avoid patterns where a computed view (or `body`) returns completely different root branches using `if/else`.
- Prefer a single stable base view, and place conditions inside sections/modifiers (`overlay`, `opacity`, `disabled`, `toolbar`, row content, etc.).
- Root-level branch swapping can cause identity churn, broader invalidation, and extra recomputation in SwiftUI.

Prefer:

```swift
var body: some View {
    List {
        documentsListContent
    }
    .toolbar {
        if canEdit {
            editToolbar
        }
    }
}
```

Avoid:

```swift
var documentsListView: some View {
    if canEdit {
        editableDocumentsList
    } else {
        readOnlyDocumentsList
    }
}
```

### 4) ViewModel initialization and dependency injection
- Create a ViewModel for each major app section or feature.
- Make ViewModels non-optional: use `@State private var viewModel: SomeViewModel`.
- Pass dependencies to the view via `init`, then pass them into the ViewModel in the view's `init`.
- **Use protocol types for dependencies** (not concrete types) to enable testability and flexibility.
- Provide default concrete implementations in the initializer signature for convenience.
- Avoid `bootstrapIfNeeded` patterns or optional ViewModels.

Example (Protocol-based dependency injection):

```swift
// View
struct FeatureView: View {
    @State private var viewModel: FeatureViewModel

    init(dataService: DataServiceContract = DataService()) {
        _viewModel = State(initialValue: FeatureViewModel(dataService: dataService))
    }

    var body: some View {
        // ...
    }
}

// ViewModel
@MainActor
@Observable
final class FeatureViewModel {
    private let dataService: DataServiceContract

    init(dataService: DataServiceContract) {
        self.dataService = dataService
    }
}

// Protocol
protocol DataServiceContract {
    func fetchData() async throws -> [Item]
}

// Concrete implementation
final class DataService: DataServiceContract {
    func fetchData() async throws -> [Item] {
        // implementation
    }
}
```

**Benefits of protocol-based DI**:
- ✅ Enables unit testing with mock implementations
- ✅ Decouples view/viewModel from concrete service implementations
- ✅ Maintains convenience with default concrete values
- ✅ Allows easy substitution of implementations (e.g., for testing, feature flags, or different environments)

**Important**: When using protocol-based dependency injection, ensure the protocol contract includes **all methods** that clients (ViewModels, Views) will use. If you add new functionality to a ViewModel that requires additional service methods, remember to update the protocol contract to include those methods.

For simple, stateless components (like reusable UI components), a ViewModel is not necessary.

### 5) Observation usage
- For `@Observable` reference types, store them as `@State` in the root view.
- Pass observables down explicitly as needed; avoid optional state unless required.

## Workflow

1) Reorder the view to match the ordering rules.
2) For major app sections/features, ensure there's a ViewModel to manage state and business logic. Use `@State`, `@Environment`, `@Query`, `task`, and `onChange` for view-level orchestration.
3) Ensure stable view structure: avoid top-level `if`-based branch swapping; move conditions to localized sections/modifiers.
4) Ensure ViewModels are non-optional: use `@State private var viewModel: SomeViewModel` initialized in `init` by passing dependencies from the view.
5) Confirm Observation usage: `@State` for root `@Observable` view models, no redundant wrappers.
6) Keep behavior intact: do not change layout or business logic unless requested.

## Notes

- Prefer small, explicit helpers over large conditional blocks.
- Keep computed view builders below `body` and non-view computed vars above `init`.
- For ViewModel architecture guidance and examples, see `references/mv-patterns.md`.

## Large-view handling

- When a SwiftUI view file exceeds ~300 lines, split it using extensions to group related helpers. Move async functions and helper functions into dedicated `private` extensions, separated with `// MARK: -` comments that describe their purpose (e.g., `// MARK: - Actions`, `// MARK: - Subviews`, `// MARK: - Helpers`). Keep the main `struct` focused on stored properties, init, and `body`, with view-building computed vars also grouped via marks when the file is long.
