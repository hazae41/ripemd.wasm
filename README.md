# ripemd.wasm

WebAssembly port of RIPEMD

```bash
npm i @hazae41/ripemd.wasm
```

[**Node Package 📦**](https://www.npmjs.com/package/@hazae41/ripemd.wasm)

## Features
- Reproducible building
- Pre-bundled and streamed
- Zero-copy memory slices

## Modules
- ripemd

## Algorithms
- RIPEMD-160 (direct and incremental)

## Usage

### Direct

```typescript
import { RipemdWasm, Memory, ripemd160 } from "@hazae41/ripemd.wasm";

// Wait for WASM to load
await RipemdWasm.initBundled();

// Data to be hashed
const hello = new TextEncoder().encode("Hello World")

// Pass to WASM
using memory = new Memory(hello)

// Grab the digest
using digest = ripemd160(memory)

console.log(digest.bytes) // Uint8Array
```

### Incremental

```typescript
import { RipemdWasm, Memory, Ripemd160Hasher } from "@hazae41/ripemd.wasm";

// Wait for WASM to load
await RipemdWasm.initBundled();

// Create a hash
using hasher = new Ripemd160Hasher()

// Data to be hashed
const hello = new TextEncoder().encode("Hello World")

// Pass to WASM
using memory = new Memory(hello)

// Update the hash with your data
hasher.update(memory)

// Grab the digest
using digest = hasher.finalize()

// Update the hash another time
hasher.update(memory)

// Grab the digest
using digest2 = hasher.finalize()

// digest !== digest2
console.log(digest.bytes)
console.log(digest2.bytes)
```

## Building

### Unreproducible building

You need to install [Rust](https://www.rust-lang.org/tools/install)

Then, install [wasm-pack](https://rustwasm.github.io/wasm-pack/installer/)

```bash
cargo install wasm-pack
```

Finally, do a clean install and build

```bash
npm ci && npm run build
```

### Reproducible building

You can build the exact same bytecode using Docker, just be sure you're on a `linux/amd64` host

```bash
docker compose up --build
```

Then check that all the files are the same using `npm diff`

```bash
npm diff
```

If the output is empty then the bytecode is the same as the one I commited

### Automated checks

Each time I release a new version on GitHub, the GitHub's CI clones the GitHub repository, reproduces the build, and throws an error if the NPM release is different. If a version is present on NPM but not on GitHub, do not use it!
