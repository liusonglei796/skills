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

### 3. `codebase-design` (Deep Module & Seam Architecture)
* **Source**: Adapted from Matt Pocock (Michael Feathers / John Ousterhout principles).
* **Location**: [`skills/codebase-design`](./skills/codebase-design)
* **Key Principles**:
  * Defines the shared vocabulary for deep modules (large capability behind small interface).
  * Seam placement and adapter design.

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
