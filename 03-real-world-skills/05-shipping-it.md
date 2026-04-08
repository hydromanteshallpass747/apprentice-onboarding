# Shipping It

## The Gap Between "It Works" and "It's Done"

Your project compiles. It runs on your machine. The features work when you test them by hand. Is it done?

No. Not even close.

The gap between "it works on my machine" and "I can hand this to someone else and it works on theirs" is one of the biggest gaps in software. This chapter is about closing it. The first half is about testing, which proves your software works. The second half is about delivery, which gets your software into other people's hands.

Testing comes first because it's where most of the work is, and it's where agents have a specific failure mode you need to know about.

## Testing: Proving It Works

### Why Testing Matters

You've been verifying your projects by hand since Phase 1. Open the app, click buttons, check the output. That works for a while. But manual testing has problems:

- You forget to test things. You test the feature you just added but forget to check that the old features still work.
- You can't test everything every time. When your project has 20 features, manually checking all of them after every change takes longer than the change itself.
- You test what you think of. The bugs that ship are the ones you didn't think to look for.

Automated tests fix all of this. You write a test once, and it runs every time. It checks what you told it to check, every time, without getting tired or forgetful. A good test suite is a living document of everything your software is supposed to do.

### Unit Tests

A **unit test** tests one small piece of code in isolation. One function, one method, one logical unit.

```
// Does this function correctly identify a high-priority issue?
fn test_is_high_priority() {
    let issue = Issue::new("Fix bug", Priority::High);
    assert!(issue.is_high_priority());
}
```

Unit tests are fast, focused, and specific. When one fails, you know exactly what broke. They're the foundation of your test suite.

**When to use them:** For every function that has logic. If a function makes decisions (if/else), transforms data, or calculates something, it should have unit tests.

**What they catch:** Logic errors, off-by-one bugs, incorrect calculations, functions that don't handle edge cases.

**How to direct your agent:**
> "Write unit tests for the search function. Test: exact match, partial match, no match, empty query, query with special characters, case sensitivity."

### Integration Tests

An **integration test** tests how multiple pieces work together. Where unit tests check that individual functions are correct, integration tests check that the functions compose correctly.

```
// Does creating an issue and then searching for it actually work end-to-end?
fn test_create_then_search() {
    let db = TestDatabase::new();
    create_issue(&db, "Fix login bug", Priority::High);
    let results = search_issues(&db, "login");
    assert_eq!(results.len(), 1);
    assert_eq!(results[0].title, "Fix login bug");
}
```

Integration tests are slower than unit tests because they involve more moving parts. But they catch a class of bugs that unit tests miss: the assumptions one module makes about another.

**When to use them:** Whenever multiple modules interact. Storage + retrieval. Parsing + processing. Input + output.

**What they catch:** Interface mismatches between modules, incorrect assumptions about data formats, state management bugs, things that work in isolation but break when combined.

**How to direct your agent:**
> "Write integration tests that exercise the full workflow: create an issue, update its status, search for it, close it, verify it appears in the closed list. Use a temporary database for each test."

### Smoke Tests

A **smoke test** is the simplest possible check that your software works at all. "Does it start? Does the main command produce output instead of crashing?" The name comes from hardware testing: plug it in and see if smoke comes out.

```
// Does the binary even run?
fn test_help_flag() {
    let output = Command::new("guild-scaffold").arg("--help").output();
    assert!(output.status.success());
}
```

Smoke tests aren't thorough. They don't check that features work correctly. They check that the software isn't fundamentally broken. A failing smoke test means something is very wrong.

**When to use them:** Always. Every binary should have at least one smoke test. They're the first thing that runs in your test suite and the first line of defense.

**What they catch:** Build failures, missing dependencies, configuration errors, crashes at startup.

**How to direct your agent:**
> "Add smoke tests for every subcommand. Each test runs the command with --help and verifies it exits successfully. Also test that running with no arguments produces a useful error message instead of crashing."

### Property-Based Tests (Prop Tests)

This is where testing gets interesting. Instead of writing specific test cases ("does search find 'login'?"), you describe *properties* that should always be true, and the test framework generates hundreds or thousands of random inputs to try to find a violation.

```
// Property: any issue we create should be retrievable by its ID.
// The framework generates random titles, priorities, and labels to try to break this.
proptest! {
    fn test_roundtrip(title in ".*", priority in 0..3u8) {
        let db = TestDatabase::new();
        let id = create_issue(&db, &title, priority.into());
        let retrieved = get_issue(&db, id);
        assert_eq!(retrieved.title, title);
    }
}
```

The framework doesn't just try random inputs once. When it finds a failing case, it *shrinks* it: it finds the simplest possible input that still triggers the failure. Instead of "your code fails on this 500-character string," you get "your code fails when the title contains a null byte."

**When to use them:** For data processing, serialization/deserialization, any function that should work for all valid inputs. Especially valuable for finding edge cases you'd never think to test by hand.

**What they catch:** Unicode handling bugs, boundary conditions, assumptions about input ranges, serialization round-trip failures. The things you'd never think to test because you'd never think of that specific input.

**How to direct your agent:**
> "Write property-based tests using proptest for the issue serialization. The property: any issue that's serialized to JSON and then deserialized should be identical to the original. Generate random titles (including empty strings, very long strings, and strings with special characters), random priorities, and random label lists."

### Formal Verification

This was introduced in the VDD chapter. Formal verification goes beyond testing to mathematical proof. Instead of "I tried 10,000 inputs and it worked," formal verification says "I can prove this is correct for all possible inputs."

In Rust, the primary tool for this is **Kani**, a model checker that proves properties about your code. It can verify things like: this arithmetic can never overflow, this array access can never be out of bounds, this function always returns a value within this range.

**When to use it:** For safety-critical code, cryptographic implementations, numeric processing, or any code where a subtle bug has serious consequences. Not every project needs formal verification. But knowing it exists puts it in your toolkit for when it matters.

**What it catches:** Arithmetic overflow, out-of-bounds access, unreachable code, logical properties that must hold for all inputs.

**How to direct your agent:**
> "Add Kani proof harnesses for the priority arithmetic. Prove that converting between priority levels and numeric values can never overflow and that the round-trip is always correct."

You'll encounter formal verification more as you advance through the guild. For now, understand the concept: some things can be proven, not just tested. That's the highest level of the confidence ladder.

## The Tautological Test Problem

Here's the agent-specific failure mode you need to know about. When you ask an agent to write tests, it will sometimes write **tautological tests**: tests that verify the code does what the code does, rather than what it *should* do.

Here's what a tautological test looks like:

```
fn test_format_priority() {
    let issue = Issue::new("Bug", Priority::High);
    // This just re-implements the function being tested
    let expected = format!("[HIGH] Bug");
    assert_eq!(issue.format(), expected);
}
```

This test passes, but it doesn't test anything meaningful. If the format function is wrong, the test is also wrong in the same way. The test is a mirror of the implementation, not an independent check against a specification.

A good test looks like this:

```
fn test_format_priority() {
    let issue = Issue::new("Bug", Priority::High);
    let formatted = issue.format();
    // Tests against independent expectations, not re-implemented logic
    assert!(formatted.contains("HIGH"), "should show priority");
    assert!(formatted.contains("Bug"), "should show title");
    assert!(formatted.starts_with("["), "should start with bracket");
}
```

This test knows what the output should look like based on the *requirements*, not the implementation. If someone changes the format function, the test catches it because the expectations are independent.

**How to catch this:** Review your agent's tests the same way you review its code. For each test, ask: "Does this test know what the right answer is independently, or is it just recomputing what the code computes?" If it's the latter, push back:

> "This test re-implements the formatting logic. The test should check against independently known correct values. Write test cases where you specify the exact expected output as string literals, based on what the format *should* be according to the design doc."

Tests need adversarial review too. A test suite full of tautologies gives you false confidence. Everything passes, but nothing is actually being verified.

## Finding Edge Cases

Edge cases are the inputs and situations that live at the boundaries. They're where bugs hide because they're what nobody thinks about during normal development.

Training yourself to think about edge cases is one of the most valuable skills you can develop. Here's a systematic way to do it:

**Empty inputs.** What happens with an empty string? An empty list? A file with no content? Zero?

**Boundary values.** The first item. The last item. Exactly at the limit. One past the limit. The maximum integer value. Negative numbers when only positives are expected.

**Special characters.** Unicode, emoji, null bytes, newlines in the middle of strings, extremely long strings, strings that look like code or commands (SQL injection, path traversal).

**Concurrent and timing issues.** What if two operations happen at the same time? What if the operation is interrupted halfway through? What if the file is being written by another process?

**State transitions.** What if you try to close an already-closed issue? What if you delete something that other things depend on? What if the config file exists but is empty?

**Resource limits.** What if there are 10,000 issues? What if the disk is full? What if a file name is 255 characters?

You won't test all of these for every project. But running through the categories mentally before you write tests (and before you direct your agent to write tests) catches the ones that matter for your specific project.

> "Before writing tests, let me describe the edge cases I want covered: empty title, title with emoji and special characters, duplicate titles, extremely long title (10,000 chars), creating an issue when the storage file doesn't exist yet, creating an issue when the storage file is corrupted JSON."

## Cross-Platform Testing

Your project works on your machine. Does it work on someone else's? Does it work on Windows if you built it on Linux? On macOS?

Cross-platform issues are real and common. File paths work differently on Windows (backslashes vs forward slashes). Line endings differ. Available system libraries differ. Terminal capabilities differ.

**Virtual machines** let you test on platforms you don't normally use. Tools like VirtualBox, UTM (for Mac), or WSL (Windows Subsystem for Linux, if you're on Windows already) let you run different operating systems on your computer.

**GitHub Actions** (or similar CI systems) can run your tests on multiple platforms automatically. You push your code, and it gets tested on Linux, macOS, and Windows without you doing anything. When you're ready to set this up, tell your agent:

> "Create a GitHub Actions workflow that runs cargo test on Ubuntu, macOS, and Windows. Run clippy and cargo fmt --check as well."

For the apprentice path, you don't need to set up CI on every project. But when you're preparing to ship something that other people will use, cross-platform testing is not optional.

## Delivery: Getting It to People

Your code is tested. Now you need to get it into someone else's hands. There are several levels of this, from simple to polished.

### "Clone My Repo and Build It"

The simplest form of distribution. Your code is on GitHub. Someone clones it and runs `cargo build --release`. This works but it requires the recipient to have Rust installed. Fine for guild members, not great for the general public.

### GitHub Releases

A step up. You build the binary for your platform, create a release on GitHub, and attach the binary as a downloadable file. Someone can download and run it without installing Rust.

Here's the process:

```
cargo build --release
```

This produces an optimized binary in `target/release/`. On GitHub, go to your repo, click "Releases," then "Create a new release." Tag it with a version number (like `v0.1.0`), write release notes, and upload the binary from `target/release/`.

**Writing good release notes:**
- Lead with what the tool does (for people who haven't seen it before)
- List what's new or changed since the last release
- Include installation instructions ("download the binary, put it in your PATH")
- Mention known issues if any
- Keep it short. A few bullet points, not an essay

> "Write release notes for v0.1.0 of the issue tracker. This is the first release. Describe what the tool does, list the features, and include installation instructions for Linux, macOS, and Windows."

### Continuous Integration (CI)

**What CI is.** Continuous Integration is a service that runs your build and tests automatically every time you push code or open a pull request. Instead of you remembering to run `cargo test` before a commit, a fresh virtual machine spins up in the cloud, clones your repo, runs the commands you specified, and reports back green (passed) or red (failed). The most common CI service for GitHub projects is **GitHub Actions**, which is built into github.com and free for public repositories.

**Why it exists.** You'll catch problems in CI that you'd never find locally:
- Code that compiles on Linux but not Windows (path separator issues, missing dependencies)
- Tests that pass on your machine but fail on a clean environment because you have something installed locally that isn't in the project
- Formatting inconsistencies between different contributors
- Dependencies that updated and broke something while you weren't looking

**The folder convention.** GitHub Actions reads workflow files from a specific magic folder: `.github/workflows/` in your repository root. Any `.yml` file in that folder is treated as a workflow. You can have multiple workflows (one for tests, one for releases, one for docs). The folder and the filename pattern are what turn an ordinary file into something GitHub runs — there's no other registration step.

**The shape of a workflow file.** Workflows are written in YAML, a data format similar in intent to JSON but using indentation instead of braces. You saw JSON back in the foundations chapter; YAML conveys the same kind of nested key-value data with different punctuation. Here's the smallest useful Rust workflow:

```yaml
name: CI

on:
  push:
  pull_request:

jobs:
  test:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v4
      - uses: dtolnay/rust-toolchain@stable
      - run: cargo build --verbose
      - run: cargo test --verbose
      - run: cargo clippy -- -D warnings
      - run: cargo fmt --check
```

The three top-level keys:

- **`name`** — a label for this workflow that shows up in the GitHub Actions tab.
- **`on`** — the triggers. This workflow runs on any push and any pull request. Other common triggers: `schedule` (cron), `workflow_dispatch` (manual button), `release` (when you publish a release).
- **`jobs`** — the actual work, organized as one or more named jobs. Each job picks a **runner** (`ubuntu-latest`, `macos-latest`, `windows-latest` are free for public repos) and runs a list of **steps** in order. Steps are either `uses` (call a pre-built action from the marketplace, like `actions/checkout@v4` which clones your repo) or `run` (execute a shell command).

If any `run` step exits non-zero, the job fails and CI reports red. Otherwise it reports green.

**Indentation matters in YAML.** Two-space indent is the convention. Tabs will break the file. One misaligned key breaks the whole workflow. The error messages are usually clear about which line is the problem.

**Setting it up for your project.** Ask your agent:

> "Create a GitHub Actions workflow file at `.github/workflows/ci.yml`. It should:
> - Run on every push and every pull request
> - Test on Ubuntu, macOS, and Windows (use a matrix strategy)
> - Run cargo build, cargo test, cargo clippy -- -D warnings, and cargo fmt --check
> - Use the stable Rust toolchain"

The agent will generate a YAML file more sophisticated than the one above (with a matrix of operating systems). Create the file, commit it, push.

**Checking the result.** Go to your repository on github.com and click the **Actions** tab at the top. You'll see a list of recent workflow runs — the most recent is the one from the push you just made. A yellow dot means pending, a green checkmark means passed, a red X means failed. Click into any run to see the step-by-step log. If a step failed, click the red step to see the exact output — Rust compile errors, test failures, and clippy lints are all there verbatim.

The debugging loop is: fail, read the log, push a fix, check the new run. You'll get fast at this once you've done it a few times.

**CI is not optional for shipped software.** If other people depend on your tool, automated testing on every change is the minimum bar. Set it up early. The earlier it catches a problem, the cheaper it is to fix.

### Cross-Compilation

Building for platforms you're not on. Rust supports this well. You can build a Windows `.exe` from a Linux machine, or a macOS binary from a Linux CI runner.

> "Extend the GitHub Actions workflow to build release binaries for Linux (x86_64), macOS (x86_64 and aarch64), and Windows (x86_64) when I push a version tag. Attach all binaries to a GitHub release automatically."

This is how mature Rust projects distribute binaries. The user downloads the one for their platform and runs it. No Rust installation required on their end.

### cargo publish

If your tool is useful to other Rust developers, you can publish it to [crates.io](https://crates.io), Rust's package registry. Then anyone can install it with `cargo install your-tool-name`.

**Publishing is optional.** Most apprentice projects should live on GitHub as source code and stop there. Publish only when the tool is mature, useful to others, and you're ready to commit to maintaining it. Once a version is on crates.io, you cannot take it back.

**Higher standards than pushing to GitHub:**

**Metadata must be complete.** Your `Cargo.toml` needs: `description`, `license`, `repository`, `keywords`, and `categories`. These show up on the crates.io listing page. Without them, `cargo publish` refuses to run.

**Documentation must exist.** A README at minimum. Ideally, doc comments on your public API so [docs.rs](https://docs.rs) can generate reference documentation automatically.

**Tests must pass.** Run `cargo test` before every publish. Shipping broken code to a public registry is bad form and **you cannot unpublish it**.

**Versions are permanent.** Once you publish version 0.1.0, you cannot delete it or modify it. You can publish 0.1.1 with a fix, but the old version stays. The only recovery mechanism is called **yank** — `cargo yank --vers 0.1.0` marks a bad version as "don't use this for new projects" but leaves it installable for anyone who already depends on it. Yanking is not deletion. Treat publishing as irreversible and plan accordingly.

**The name is globally unique and first-come.** `cargo publish` claims the crate name forever. Pick something you'll be happy with in five years.

#### Step-by-step publishing walkthrough

Assume you have a mature tool ready to ship. Here's the full path from zero to published.

**1. Create a crates.io account.** Go to [crates.io](https://crates.io) and click **Log in with GitHub** in the top right. This signs you in via GitHub OAuth — you don't create a separate password. The first sign-in creates your account automatically.

**2. Verify your email address.** crates.io requires a verified email before you can publish. Click your profile → **Account Settings** → add an email address if none is listed → click the verification link in the email that arrives. Publishing will fail with a clear error if this step is skipped.

**3. Generate an API token.** On crates.io, go to **Account Settings** → **API Tokens** → **New Token**. Name it something like "my laptop" and leave the default scopes (allow publish-update). Click **Create**. The token is shown once — copy it immediately.

**4. Log cargo into crates.io with the token.**
```
cargo login <paste-your-token>
```

This writes the token to `~/.cargo/credentials.toml`. Cargo will use it for all future publishes from this machine. You only do this once per computer. If you ever lose the token, revoke it on crates.io and generate a new one.

**5. Fill out your Cargo.toml metadata.** Open `Cargo.toml` and make sure the `[package]` section has all the required fields:

```toml
[package]
name = "my-tool"
version = "0.1.0"
edition = "2024"
description = "A one-sentence summary of what the tool does"
license = "MIT OR Apache-2.0"
repository = "https://github.com/YOUR-USERNAME/my-tool"
readme = "README.md"
keywords = ["cli", "productivity"]
categories = ["command-line-utilities"]
```

Pick keywords and categories carefully — crates.io uses them for search. You can find the full category list at [crates.io/categories](https://crates.io/categories).

**6. Dry run.** Always do this before publishing:
```
cargo publish --dry-run
```

This packages the crate as if you were publishing, runs the checks, and reports any problems without actually sending anything to crates.io. Fix every warning. Common issues: missing README file referenced in Cargo.toml, files larger than the size limit, unrelated files accidentally included.

**7. Publish.**
```
cargo publish
```

It uploads the crate tarball, crates.io indexes it, and within a minute `cargo install my-tool` works for anyone. Your crate page appears at `crates.io/crates/my-tool`, and docs.rs starts building your reference docs automatically.

**8. Tag the release in git.** Create and push a git tag that matches the version you just published:
```
git tag v0.1.0
git push origin v0.1.0
```

This makes it easy for future-you and your users to find the exact code that corresponds to any published version.

#### Publishing follow-up versions

To publish a new version later, bump the `version` field in `Cargo.toml` (following semantic versioning), commit, `cargo publish --dry-run`, `cargo publish`, tag, push. That's the loop.

### The Packaging Checklist

Before you ship anything, run through this:

- **Does it build from a clean checkout?** Clone a fresh copy and build it. Don't rely on anything in your local environment that isn't in the repo.
- **Do all tests pass?** `cargo test`. No skipping, no ignoring failures.
- **Does clippy pass?** `cargo clippy -- -D warnings`. Clean code ships cleaner.
- **Does CI pass?** If you have GitHub Actions, the green checkmark is your baseline. Don't ship if CI is red.
- **Is there a README?** What does the tool do, how do you install it, how do you use it. Write it for someone who's never seen your project before.
- **Are there release notes?** What changed since the last version. What's new, what's fixed, what's known-broken.
- **Is the version number correct?** Follow semantic versioning: major.minor.patch. Bug fixes increment patch. New features increment minor. Breaking changes increment major.
- **Are secrets excluded?** No API keys, no tokens, no `.env` files in the repo. Check your `.gitignore`.
- **Does it work on a fresh machine?** This is what VMs and CI are for. If you only tested on your machine, you haven't tested enough.

## Exercises

1. Take your issue tracker from Phase 2 and add a complete test suite. Start with smoke tests (does it run?), then unit tests for each piece of logic, then integration tests for workflows (create → update → search → close). Run `cargo test` and make sure everything passes. Then ask your agent: "review these tests for tautological testing." Fix any it finds.

2. Write property-based tests for your issue tracker's serialization. Use proptest to generate random issues and verify the round-trip: serialize to JSON, deserialize back, compare to original. What edge cases does proptest find that your hand-written tests missed?

3. Run your issue tracker's tests on a different platform than you developed on. If you're on Linux, test on macOS or Windows (via VM, WSL, or GitHub Actions). Document what broke and what you had to fix. Cross-platform issues are easier to find than to predict.

4. Package one of your projects for distribution. Create a GitHub release with a built binary. Write a README that explains what it is, how to install it, and how to use it. Have someone else (another guild member, a friend) try to use it from just the README and release binary. Their experience will teach you more about shipping than any guide can.

5. Set up a GitHub Actions workflow that runs your full test suite (tests, clippy, formatting) on every push. Make it test on at least two platforms. Watch it catch something you missed locally. This is the beginning of CI/CD, and it's how professional projects stay healthy.
