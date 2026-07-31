# 🖥️ Systems Programmer — Skills Guide

Systems programmers work close to the hardware — building operating system utilities, embedded firmware, high-performance runtimes, compilers, and security tools. This guide covers Rust, C/C++, embedded, debugging, and shell scripting.

---

## 🗺️ Skill Map at a Glance

| Concern | Top Skills |
|---|---|
| Languages | `@rust-pro`, `@c-pro`, `@cpp-pro`, `@systems-programming-rust-project` |
| Async / Concurrency | `@rust-async-patterns`, `@go-concurrency-patterns` |
| Memory & Safety | `@memory-safety-patterns`, `@constant-time-analysis`, `@memory-forensics` |
| Debugging | `@gdb-cli`, `@binary-analysis-patterns` |
| Embedded / Hardware | `@arm-cortex-expert`, `@firmware-analyst` |
| OS / Shell | `@linux-shell-scripting`, `@bash-pro`, `@bash-defensive-patterns`, `@posix-shell-pro` |
| Build Systems | `@bazel-build-optimization` |
| Security | `@anti-reversing-techniques`, `@protocol-reverse-engineering`, `@reverse-engineer` |
| Windows | `@powershell-windows`, `@windows-shell-reliability` |

---

## 🦀 Rust

### `@rust-pro`
Production Rust: ownership, lifetimes, traits, error handling, crates.
```
@rust-pro Build a high-performance event QR ticket validator in Rust:
Requirements:
- Parse QR payload (JWT-like: header.claims.signature, base64url encoded)
- Validate HMAC-SHA256 signature against shared secret
- Check claims: expiry (exp), event_id match, already_used flag in Redis
- Return: Valid(ticket_id) | Invalid(reason)

Performance target: < 1ms per validation, 10,000 req/s single-threaded
Crates: tokio, redis-rs, hmac, sha2, base64, serde, thiserror
Include: proper error types with thiserror, unit tests for each failure case.
```

### `@systems-programming-rust-project`
Structuring a larger Rust project.
```
@systems-programming-rust-project Structure a Rust workspace for our events infrastructure:
Crates:
- events-core: domain types (Event, Registration, Ticket)
- events-api: Axum HTTP server
- events-worker: background job processor (Tokio)
- events-cli: admin CLI tool (clap)
- events-common: shared utilities (errors, tracing, config)

Workspace Cargo.toml, dependency sharing, feature flags,
build profiles for dev (fast compile) and release (optimized).
```

### `@rust-async-patterns`
Tokio async runtime, channels, tasks, cancellation.
```
@rust-async-patterns Design an async event processing pipeline in Rust:

Pipeline stages (connected via channels):
1. Ingest: tokio::net::TcpListener → accept connections
2. Parse: decode protobuf messages from raw bytes
3. Validate: check auth + business rules
4. Store: insert to PostgreSQL via sqlx
5. Notify: push to Redis pub/sub for downstream consumers

Requirements:
- Bounded mpsc channels for backpressure (capacity: 1000 per stage)
- Graceful shutdown: CancellationToken propagated to all tasks
- Metrics: tokio::sync::atomic counters for throughput
- Error isolation: one bad message doesn't crash the pipeline
```

### `@memory-safety-patterns`
Safe Rust and safe C++ patterns; sanitizer usage.
```
@memory-safety-patterns Review this unsafe Rust block and make it safe:

unsafe {
    let raw_ptr = Box::into_raw(Box::new(Event { id: 1, name: "Hackathon".to_string() }));
    let event_ref = &*raw_ptr;
    println!("{}", event_ref.name);
    // BUG: raw_ptr is never freed!
}

Identify: memory leak, dangling pointer risk.
Rewrite: using safe Rust (Arc, Box, or just stack allocation).
Also: when IS unsafe justified? Give 3 legitimate use cases.
```

### `@constant-time-analysis`
Timing-safe operations for security-critical code.
```
@constant-time-analysis Audit our ticket validation code for timing side channels:
Current code compares HMAC signatures with == operator.

Explain:
- Why string comparison with == is a timing oracle
- How an attacker exploits it
- Show the constant-time comparison implementation in Rust (subtle crate)
- Show it in Python (hmac.compare_digest)
- Show it in Node.js (crypto.timingSafeEqual)
```

---

## 🔵 C and C++

### `@c-pro`
C systems programming: pointers, manual memory, POSIX APIs.
```
@c-pro Write a C program for our event venue check-in kiosk:
- Read QR code from USB scanner (as keyboard input via /dev/input)
- Parse the ticket ID from QR payload
- HTTP GET to our validation API (use libcurl)
- Green LED + sound for valid, Red LED + error sound for invalid
- GPIO via /sys/class/gpio on Raspberry Pi
- Loop forever, graceful exit on SIGTERM
```

### `@cpp-pro`
Modern C++ (C++17/20): RAII, smart pointers, STL, templates.
```
@cpp-pro Implement a lock-free SPSC (single-producer, single-consumer) queue in C++:
- Template: SPSCQueue<T, Capacity>
- Cache-line aligned head and tail (avoid false sharing)
- Memory ordering: relaxed for loads, release/acquire for synchronization
- try_push / try_pop (non-blocking)
- Benchmark: compare with std::queue<T> + std::mutex baseline
Use Google Benchmark. Measure throughput at 1M items.
```

---

## 🔬 Debugging

### `@gdb-cli`
GDB debugger for C, C++, and Rust binaries.
```
@gdb-cli Our QR validator crashes with SIGSEGV under high load.
Walk me through GDB debugging:
1. Enable core dumps (ulimit -c unlimited)
2. Load core: gdb ./validator core
3. Print backtrace: bt full
4. Inspect locals and registers at crash frame
5. Identify: null pointer? Stack overflow? Use-after-free?
6. Set conditional breakpoints to catch the condition before crash
7. Use watchpoints on suspected corrupted memory address
```

### `@binary-analysis-patterns`
Static and dynamic binary analysis.
```
@binary-analysis-patterns Analyze an event check-in binary we received from a vendor:
We don't have the source code.
Tools to use:
1. file + strings: initial recon
2. objdump / readelf: symbol table, imports
3. strace / ltrace: system calls and library calls at runtime
4. Ghidra: decompile and analyze main logic
5. Identify: what API endpoints does it call? Any hardcoded credentials?
```

---

## 🔧 Embedded & Firmware

### `@arm-cortex-expert`
Bare-metal ARM Cortex-M programming.
```
@arm-cortex-expert Design a hardware event check-in badge using STM32F4:
Hardware:
- OLED display (SSD1306, I2C)
- NFC reader (PN532, SPI)
- RGB LED (GPIO)
- Battery: LiPo with charger IC

Firmware requirements:
- Read NFC tag → extract ticket ID
- Validate against cached allowlist (stored in Flash)
- Display result on OLED
- LED: green (valid) / red (invalid) / blue (already used)
- Low power: sleep between scans, wake on NFC interrupt

Provide: startup code, peripheral drivers, main state machine.
```

### `@firmware-analyst`
Analyzing existing firmware for security and functionality.
```
@firmware-analyst Analyze the firmware from our venue access control system:
We extracted the firmware via JTAG.
Tasks:
1. Identify: RTOS (FreeRTOS? bare-metal?)
2. Find: network communication code (any hardcoded endpoints?)
3. Look for: hardcoded admin passwords or backdoors
4. Find: OTA update mechanism (is it signed? can we forge an update?)
5. Identify: any memory corruption vulnerabilities
Tools: Binwalk, Ghidra, Radare2
```

---

## 🐚 Shell & OS Scripting

### `@bash-pro` / `@linux-shell-scripting`
Production Bash scripting.
```
@bash-pro Write a production-grade deployment script for our API server:
Requirements:
- Check prerequisites: node, pm2, postgres connection
- Stop old process: pm2 stop api (wait for graceful shutdown, max 30s)
- Deploy: git pull, npm ci, npm run build
- Migrate: run prisma migrate deploy
- Start: pm2 start ecosystem.config.js
- Health check: curl localhost:3000/health every 5s, 12 retries
- Rollback: if health check fails, restore previous build
- Logging: timestamped log to /var/log/api-deploy.log
- Alert: curl Slack webhook on failure

POSIX-compatible, set -euo pipefail throughout.
```

### `@bash-defensive-patterns`
Safe bash: error handling, quoting, traps.
```
@bash-defensive-patterns Review this deployment script for bash safety issues:
[paste script]
Check for:
- Missing set -e, -u, -o pipefail
- Unquoted variables (word splitting risk)
- Missing error handling on critical commands
- Temp file cleanup on exit (trap)
- Race conditions
- Input validation if script takes arguments
```

### `@posix-shell-pro`
POSIX sh compatible scripts (works on Alpine, BusyBox, macOS).
```
@posix-shell-pro Rewrite our bash deploy script as POSIX sh:
Must run on: Ubuntu 22.04, Alpine 3.18, macOS 14
Remove bash-specific: arrays, [[]], $RANDOM, process substitution
Use POSIX equivalents: [ ], printf, $()
Test: shellcheck --shell=sh
```

### `@powershell-windows`
PowerShell scripting for Windows systems.
```
@powershell-windows Write a PowerShell script to deploy our .NET API on Windows Server:
- Stop IIS app pool gracefully
- Copy build artifacts to C:\inetpub\events-api
- Update appsettings.json connection strings from environment
- Run EF migrations
- Start IIS app pool
- Health check: Invoke-WebRequest
- Event log on success/failure
```

---

## 🏗️ Build Systems

### `@bazel-build-optimization`
Bazel for fast, reproducible builds.
```
@bazel-build-optimization Set up Bazel for our C++ + Rust monorepo:
- BUILD files for each package
- Cross-language dependencies: Rust crates depend on C++ proto stubs
- Remote caching: connect to BuildBuddy
- Remote execution: distribute heavy builds
- CI integration: only rebuild changed targets
Benchmark: current make build time vs Bazel with caching.
```

---

## 🛡️ Security

### `@anti-reversing-techniques`
Protect binaries from reverse engineering.
```
@anti-reversing-techniques Our event validation binary contains our crypto key.
Apply anti-reversing techniques to protect it:
- Strip debug symbols in release build
- Obfuscate the key (not hardcode as string literal)
- Control flow flattening (OLLVM)
- Anti-debugger checks (ptrace detection)
- String encryption at compile time
Note: we understand security through obscurity has limits — explain tradeoffs.
```

---

## 🔗 Complete Systems Programmer Prompt Chain

```
1️⃣  @rust-pro (or @c-pro / @cpp-pro)
    "Design and implement core high-performance component"

2️⃣  @rust-async-patterns
    "Add async I/O, concurrent processing, backpressure"

3️⃣  @memory-safety-patterns
    "Review for memory safety: leaks, use-after-free, data races"

4️⃣  @constant-time-analysis
    "Audit crypto operations for timing side channels"

5️⃣  @gdb-cli
    "Debug crashes with core dumps and conditional breakpoints"

6️⃣  @binary-analysis-patterns
    "Analyze third-party or compiled binaries"

7️⃣  @bash-defensive-patterns
    "Harden deployment and operational scripts"

8️⃣  @bazel-build-optimization
    "Speed up build pipeline with remote caching"
```

---

## 💡 Pro Tips for Systems Programmers

1. **`@rust-pro` + `@memory-safety-patterns`** before any `unsafe` block
2. **`@constant-time-analysis`** for ANY code that touches secrets or tokens
3. **`@bash-defensive-patterns`** on every shell script — `set -euo pipefail` is non-negotiable
4. **`@gdb-cli` before adding debug prints** — learn the debugger first
5. **`@rust-async-patterns`** for understanding bounded channels and backpressure — critical for pipeline reliability
