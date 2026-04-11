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
