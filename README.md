# tw6-r-01
Monitor and record  and document progress, highlight any issues or challenges


## cargo-generate:
Both WebAssembly (Wasm) and Embedded Rust are fields where cargo-generate is almost a requirement because the "boilerplate" (the setup code) is too complex to write from scratch every time.
Since you're starting with Wasm and moving to Embedded later, here is how you’ll use that tool you just installed:
### 1. WebAssembly Workflow
In Wasm, you aren't just writing Rust; you’re writing Rust that needs to "talk" to JavaScript. The templates set up the linkers and build scripts so your Rust functions can be called in a browser.
**The standard starting point:**
```bash
cargo generate --git https://github.com/rustwasm/wasm-pack-template

```
 * **What it gives you:** A pre-configured Cargo.toml with wasm-bindgen (the bridge to JS) and a folder structure ready for wasm-pack.
 * **Next step:** You’ll likely use wasm-pack build to turn your Rust into a .wasm file that a website can use.
### 2. Embedded Rust Workflow
This is where cargo-generate becomes a lifesaver. In embedded, you have to tell Rust exactly which chip you are using, how much RAM it has, and where the memory starts.
**The "Standard" Cortex-M Template:**
```bash
cargo generate --git https://github.com/rust-embedded/cortex-m-quickstart

```
 * **What it gives you:** * **memory.x:** A file where you define your chip's memory layout.
   * **.cargo/config.toml:** Settings that tell Rust to compile for a specific ARM architecture (like thumbv7em-none-eabihf) instead of your computer's CPU.
   * **Panic handling:** Embedded systems don't have a screen to print errors, so it sets up "panic" behavior (like looping forever or triggering a breakpoint).
### Why use cargo-generate for these specifically?
| Feature | WebAssembly | Embedded |
|---|---|---|
| **Target CPU** | wasm32-unknown-unknown | e.g., thumbv6m-none-eabi |
| **Standard Library** | Limited (No file system) | Often no_std (No OS at all) |
| **Complexity** | High (JS/Rust glue code) | Very High (Hardware registers/Memory maps) |
| **The Benefit** | Skips 20 mins of config. | Skips hours of debugging linker errors. |
### Quick Tip for 2026
If you end up working with **ESP32** chips (very popular for hobbyists), they have their own specialized generator that is much more modern:
cargo generate esp-rs/esp-idf-template (for standard apps) or esp-rs/esp-generate (for bare-metal).
Which one are you diving into first—are you looking to build a web-based tool or start flashing some hardware?

## Flashing hardware:

Welcome to the "Bare Metal" world! Flashing hardware with Rust has become incredibly streamlined as of 2026. Since you're ready to start, here is your roadmap for getting code onto a physical chip.
## 1. The "Must-Have" Tool: probe-rs
In the past, you needed separate tools to compile, flash, and debug (like openocd or gdb). Now, the Rust community has standardized on **probe-rs**. It is a one-stop shop for flashing and getting logs back from your chip.
**Install it now:**
```bash
cargo install probe-rs-tools --locked

```
 * **Why:** It allows you to run cargo run and have the code automatically flash to your hardware and open a terminal to show you println!-style logs from the chip.
## 2. Choosing Your Hardware (2026 Favorites)
If you don't have a board yet, these are the current top picks for Rust:
| Hardware | Chip | Why it's great for Rust |
|---|---|---|
| **Raspberry Pi Pico 2** | **RP2350** | The "Gold Standard" for beginners. Massive community support and high-performance PIO pins. |
| **ESP32-C6 / S3** | **ESP32** | Best for IoT. Rust support is native and excellent, especially for Wi-Fi and Bluetooth. |
| **STM32 Nucleo** | **Cortex-M** | The industry standard. Very robust and reliable for professional "hard" real-time learning. |
## 3. Your First "Blinky" Project
Once your hardware arrives, you'll use cargo-generate to pull the specific template for that chip. For example, if you got a **Raspberry Pi Pico**:
 1. **Generate the project:**
   ```bash
   cargo generate --git https://github.com/rp-rs/rp2040-project-template
   
   ```
 2. **Connect your hardware** via USB.
 3. **Flash it:**
   ```bash
   cargo run
   
   ```
## 4. Key Concepts You'll Encounter
 * **no_std:** Most embedded Rust doesn't use the Standard Library (no std::collections, no std::fs) because there is no Operating System. You'll use the core crate instead.
 * **PAC (Peripheral Access Crate):** The lowest level of code that knows exactly where the "buttons" and "lights" are on the chip.
 * **HAL (Hardware Abstraction Layer):** A friendlier layer that lets you say led.set_high() instead of writing a hex value to a specific memory address.
### A Note on "The Linker"
The biggest hurdle for beginners is usually the **linker script** (memory.x). This file tells Rust, *"Hey, the Flash memory starts at address 0x08000000 and is 256KB big."* **The Good News:** Because you are using cargo-generate templates, this file is usually filled out for you!
**Do you already have a piece of hardware on your desk, or are you looking to buy one today?**
