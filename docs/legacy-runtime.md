# Legacy ARM32 runtime

This repository builds and stores the compatibility Node.js runtime used by Tailnet TV on older LG webOS devices with 32-bit ARM EABI5 soft-float userspace.

## Profile

- Profile: `legacy-arm32-softfp-static`
- Node.js: `16.20.2`
- Target: ARMv7 / 32-bit userspace
- Floating-point calling ABI: softfp
- FPU: VFPv3
- Linkage: fully static
- WebAssembly: disabled
- Runtime must not depend on a target dynamic loader.

The target class was derived from legacy LG webOS hardware where the kernel is AArch64-capable but the webOS userspace is 32-bit ARM EABI5 soft-float.

## Validation

The workflow validates that the resulting binary:

1. is ELF32 for ARM,
2. uses the soft-float ABI,
3. has no requested dynamic program interpreter,
4. starts under `qemu-arm`,
5. reports Node `v16.20.2`, `arm`, and `linux`,
6. provides the modern APIs currently required by Tailnet TV, including `node:fs/promises` and `crypto.randomUUID`.

QEMU validation is not a replacement for a physical LG webOS smoke test.

## Publishing

Pull requests only build and validate the runtime.

Persistent publication is manual through the **Legacy ARM32 runtime** workflow using `workflow_dispatch`. A publication creates or updates a GitHub Release tagged like:

```
runtime-node16-arm32-softfp-r1
```

The release contains:

- `node-v16.20.2-linux-armv7-softfp-static.tar.gz`
- the matching `.sha256`
- `runtime-manifest.json`

Application releases should pin both the runtime release tag and SHA-256 instead of relying on GitHub Actions artifact retention.

## Safety

This runtime is private to the Tailnet TV application/service. It must never replace `/usr/bin/node`, system loaders, or other webOS root filesystem components.
