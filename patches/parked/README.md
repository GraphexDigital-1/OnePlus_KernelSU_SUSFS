# Parked patches (NOT applied by the build)

The CI builds the **GKI kernel tree** (`kernel_platform/common`, synced from
the manifest). That tree does **not** contain `drivers/gpu/msm/`: on these
devices the Adreno/kgsl driver ships as a **prebuilt vendor module**, so
patches touching `drivers/gpu/msm/*` cannot be applied here — `patch` fails
with "can't find file to patch" and aborts the "Apply Local Patches" step.

These patches are kept for reference and for builds of a *full* kernel tree
that does include `drivers/gpu/msm` (e.g. the local OnePlusOSS checkout with
the pre-split layout, where they were verified to apply and compile):

- `0004-kgsl-skip-wait-for-signaled-fences.patch`  (Kirisakura b693fc0ed5a0)
- `0008-kgsl-fenced-writes-out-of-spinlock.patch`  (Kirisakura b865c34e1894)
- `0009-kgsl-low-prio-worker.patch`                (Kirisakura 4fa036097af8)

Notes:
- They are hand ports to 5.10 (gen7/a730 + a6xx) and were compile-tested
  with clang-r416183b against a tree that has the driver.
- `0009` is adapted to the 5.10 `kthread_worker` based event code and also
  wires the low priority event dispatch (the 4.14 original left it unused).
- Subdirectories are not matched by the `patches/*.patch` glob used by the
  *Apply Local Patches* step, so nothing here runs during a build.
