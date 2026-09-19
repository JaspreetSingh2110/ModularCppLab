# C++ Design Patterns Learning Plan

Track progress by changing `[ ]` → `[x]` when a task is done.

**Goal:** Modern C++ fluency → design patterns practice → sample multi-library project (blueprint for work refactor).

**Baseline:** Comfortable through C++11  
**Target dialect:** C++17/20 day-to-day; C++23/26 awareness (project CMake uses C++26)

**Office constraint (confirmed):** Final library is **shipped as a binary to external consumers** → public surface should be a **stable C API** (`extern "C"`, opaque handles). C++ classes stay **inside** the libs; optional thin C++ wrappers may call the C API only.

---

## Repo layout

```
ModularCppLab/
├── LEARNING_PLAN.md          ← progress tracker (this file)
├── CMakeLists.txt            ← root build (options below)
├── app/playground/           ← scratch / hello executable
├── lessons/                  ← Phase A–C practice code
│   ├── phase_a/              ← modern C++ (a1…a6)
│   ├── phase_b/              ← design patterns
│   └── phase_c/              ← integration drills
├── sample/                   ← Phase D multi-lib blueprint
│   ├── app/                  ← links BigLib only
│   └── libs/
│       ├── core/
│       ├── feature_a/
│       ├── feature_b/
│       └── big_lib/          ← Facade / umbrella
├── tests/                    ← unit tests (from A6 onward)
└── docs/notes/               ← optional written notes
```

**CMake options**

| Option | Default | Purpose |
|--------|---------|---------|
| `DP_BUILD_LESSONS` | ON | Build lesson targets under `lessons/` when a lesson has `CMakeLists.txt` |
| `DP_BUILD_SAMPLE` | OFF | Build Phase D sample (`sample/`) |
| `DP_BUILD_TESTS` | OFF | Build `tests/` |

Lesson folders stay empty until we start that topic. Each lesson gets its own `CMakeLists.txt` + sources when we begin it; the root auto-picks it up.

---

## How we work

- Complete one task at a time (code + short explanation).
- Mark the task `[x]` here when done.
- Put lesson code in the matching folder under `lessons/` (e.g. A1 → `lessons/phase_a/a1_cpp14/`).
- Do not start Phase D until Phases A–C feel solid.

---

## Phase A — Modern C++ refresh (from C++11)

### A1. C++14 essentials
- [ ] Generic lambdas
- [ ] `std::make_unique`
- [ ] Relaxed `constexpr` / return type deduction
- [ ] Mini exercise: rewrite a small C++11 snippet using 14 features

### A2. C++17 — core (priority)
- [ ] `std::optional`, `std::variant`, `std::string_view`
- [ ] Structured bindings
- [ ] `if constexpr` and fold expressions (basic)
- [ ] Nested namespaces (`namespace a::b`)
- [ ] `std::filesystem` (basic usage)
- [ ] Mini exercise: API that returns `optional` / uses `string_view` safely

### A3. Ownership & API hygiene (critical for libraries)
- [ ] Value semantics vs pointers/references/views
- [ ] Rule of 0 / 3 / 5 and move semantics (deepen)
- [ ] `unique_ptr` vs `shared_ptr` — when each is justified
- [ ] `[[nodiscard]]`, `noexcept`, `explicit`
- [ ] Header hygiene: forward declarations, what belongs in `.h` vs `.cpp`
- [ ] Mini exercise: class with clear ownership, no unnecessary `shared_ptr`

### A4. C++20 — core (priority)
- [ ] Concepts + `requires` (write one constrained template)
- [ ] `std::span`
- [ ] Ranges basics (`std::ranges`, simple views)
- [ ] `std::format`
- [ ] Three-way comparison `<=>` (when useful)
- [ ] Coroutines — conceptual only (what/when; no deep dive yet)
- [ ] Modules — awareness only
- [ ] Mini exercise: interface via concept *or* abstract base; compare both

### A5. C++23 / C++26 — awareness (updated engineer)
- [ ] `std::expected` for recoverable errors
- [ ] `std::print` / `std::println`
- [ ] Skim other 23 features (`mdspan`, `move_only_function`, generator)
- [ ] Note toolchain support (what your compiler actually has)
- [ ] Mini exercise: function returning `std::expected` (if available) or a simple Result type

### A6. Tooling for library work
- [ ] CMake: `add_library`, `PUBLIC` / `PRIVATE` / `INTERFACE` link & include
- [ ] Warnings + sanitizers mindset (ASan/UBSan)
- [ ] Unit test runner chosen (Catch2 or GoogleTest) — hello test builds
- [ ] Mini exercise: one static lib + one executable that links it

**Phase A complete when:** you can write small C++17/20 APIs with clear ownership and build a tiny lib+app with CMake.

---

## Phase B — Design patterns (C++ practice)

Learn in this order (tied to multi-lib refactor). Each item = concept + coded example in this repo.

### B1. Foundations
- [ ] SOLID overview (focus: S, O, D)
- [ ] Coupling vs cohesion; dependency direction
- [ ] “Library seam” — what makes a good boundary

### B2. Core patterns for your work goal
- [ ] Dependency Inversion (interfaces / abstract bases / concepts)
- [ ] Facade (umbrella API over internals)
- [ ] Adapter (wrap legacy / migrate gradually)
- [ ] Strategy (swappable behavior)
- [ ] Factory Method (and when Abstract Factory helps)
- [ ] Observer (or callback-based equivalent)
- [ ] PIMPL (C++ idiom for stable/public headers)

### B3. Secondary patterns (as needed)
- [ ] Decorator
- [ ] Command
- [ ] Builder
- [ ] State
- [ ] Singleton — know it; prefer not to use casually

### B4. Pattern synthesis
- [ ] Short write-up: which patterns map to “small libs → one big lib”
- [ ] Sketch dependency diagram for a fictional domain (boxes = libs)

**Phase B complete when:** you can implement B2 patterns from memory and explain *why* each helps library splits.

---

## Phase C — Integration drills

- [ ] Combine Strategy + Factory behind one interface
- [ ] Combine Adapter + Facade over a “messy” fake legacy module
- [ ] PIMPL + clear public header with no private includes leaking
- [ ] Review: list anti-patterns seen in tightly coupled single-folder code

**Phase C complete when:** you can compose 2–3 patterns without forcing them.

---

## Phase D — Sample project (together)

Blueprint for your work refactor: multiple small libraries → one umbrella library → app.

Target shape for shipped product:

```
[External app]  --includes-->  big_lib C API (.h only, extern "C")
       |                         |
       +------ links binary -----+
                                 v
                    BigLib wrappers (C → C++)
                                 |
              +------------------+------------------+
              v                  v                  v
           feature_a          feature_b           core
           (C++ only)         (C++ only)        (C++ only)
```

- [ ] Agree on a small domain (e.g. document pipeline, sensor hub, game services — TBD)
- [ ] Define library map: `Core`, feature libs, `BigLib` (C Facade), `app`
- [ ] Implement Core interfaces only (C++)
- [ ] Implement 2–3 feature libraries (C++)
- [ ] Implement BigLib **C API** + opaque handles + `extern "C"` wrappers
- [ ] Ensure **no C++ class headers** are required by external `app`
- [ ] CMake: only C API headers are `PUBLIC` install/export; C++ headers `PRIVATE`
- [ ] App links only BigLib and includes only the C header(s)
- [ ] Add tests at library boundaries (+ smoke test of C API)
- [ ] Retrospective: map lessons back to office shipped-library workflow

**Phase D complete when:** sample builds, demos the C-boundary design, and you can explain how to apply it at work.

---

## Progress log

| Date | Completed | Notes |
|------|-----------|-------|
| | | |

---

## Current focus

**Next up:** Phase A1 — C++14 essentials

When you finish a task, say “mark X complete” (or edit the checkbox here), and we’ll start the next one.
