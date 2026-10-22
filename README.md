# Agent Skills

Curated AI agent skills for [Antigravity](https://github.com), Claude Code, and other AI coding assistants.

## 📦 Included Skills

### 1. `tdd` (Test-Driven Development)
* **Source**: Adapted from [Matt Pocock](https://github.com/mattpocock/skills) with custom Go testing integrations.
* **Location**: [`skills/tdd`](./skills/tdd)
* **Key Principles**:
  * **Red-Green Loop**: Write exactly one failing test first, then write the minimal implementation to pass it.
  * **Vertical Slices (Tracer Bullets)**: One seam, one test, one minimal implementation per cycle. Avoid bulk tests before code.
  * **Seam-Oriented Testing**: Test through public interfaces / seams rather than internal implementation details.
  * **Go Integration**: Explicitly links with `golang-testing` for table-driven tests, subtests, and concurrency-safe assertions in Go codebases.

### 2. `golang-testing` (Production-Ready Go Testing)
* **Source**: Samber Go engineering best practices.
* **Location**: [`skills/golang-testing`](./skills/golang-testing)
* **Key Principles**:
  * Table-driven tests with named subtests (`t.Run`).
  * Parallel test execution (`t.Parallel()`).
  * `testify/require` and `testify/assert` idioms.
  * Goroutine leak detection with `uber-go/goleak`.
  * Integration tests separated by `//go:build integration`.
  * Mocking interfaces at system boundaries, not concrete structs.

### 3. `golang-lint` (Production-Grade Go Linting & Static Analysis)
* **Source**: Samber Go engineering best practices.
* **Location**: [`skills/golang-lint`](./skills/golang-lint)
* **Key Principles**:
  * Production-ready `.golangci.yml` configuration template with 40+ enabled linters.
  * Catches unclosed resources (`bodyclose`, `sqlclosecheck`), unhandled errors (`errcheck`, `nilerr`), and security flaws (`gosec`).
  * Enforces proper `//nolint:linter // justification` suppression rules (`nolintlint`).
  * Supports automated fixing with `golangci-lint run --fix` and formatting via `gofumpt`/`goimports`.
  * Multi-agent legacy codebase incremental cleanup strategy.

### 4. `golang-benchmark` (Performance Measurement & Statistical Profiling)
* **Source**: Samber Go engineering best practices.
* **Location**: [`skills/golang-benchmark`](./skills/golang-benchmark)
* **Key Principles**:
  * Micro-benchmarking with Go 1.24+ `b.Loop()` and `b.ReportAllocs()`.
  * Statistical rigor using `benchstat` with confidence intervals & p-value testing.
  * Direct profiling integration (`pprof` CPU/memory profiles, execution `trace`).
  * Compiler optimization analysis (escape analysis, inlining decisions).
  * CI regression detection to prevent performance degradation on pull requests.

### 5. `codebase-design` (Deep Module & Seam Architecture)
* **Source**: Adapted from Matt Pocock (Michael Feathers / John Ousterhout principles).
* **Location**: [`skills/codebase-design`](./skills/codebase-design)
* **Key Principles**:
  * Defines the shared vocabulary for deep modules (large capability behind small interface).
  * Seam placement and adapter design.

### 6. `docx` (Word Document Creation, Editing & Analysis)
* **Source**: Anthropic Official Agent Skills (`anthropics/skills`) with Custom Template Guidelines.
* **Location**: [`skills/docx`](./skills/docx)
* **Key Principles**:
  * **Template-First Modification**: Strict adherence to modifying existing templates in-place, preserving page margins, running headers/footers, styles, and section breaks. In-place substitution of placeholders.
  * **Lossless XML Editing**: Modifies `word/document.xml` directly to maintain full fidelity of styles, headers/footers, and page layouts.
  * **Run Coalescing**: Uses `merge_runs.py` to merge fragmented adjacent text runs so search-and-replace never misses matches.
  * **Track Changes & Comments**: Native redlining and comment insertion with multi-file XML relationship synchronization.
  * **Schema Validation**: Automated XSD validation against ECMA/ISO Office Open XML standards with auto-repair.

---

## 🚀 Installation & Usage

### Global Installation (Antigravity)
Copy the skills to your global Antigravity configuration directory:

```bash
mkdir -p ~/.gemini/config/skills
cp -r skills/* ~/.gemini/config/skills/
```

### Workspace Installation
To enable these skills for a specific repository:

```bash
mkdir -p .agents/skills
cp -r skills/* .agents/skills/
```
