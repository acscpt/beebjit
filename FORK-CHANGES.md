# Fork changes

- **2026-04-14**: Add `-log-stderr` flag routing diagnostic log output to
  stderr instead of stdout. Default behaviour unchanged.

- **2026-04-15**: Move Linux mmap constants to the 0x70000000 band,
  mirroring upstream's Windows fix (f3160a3b), to avoid intermittent
  address-space collisions at startup.
  
- **2026-04-26**: Add Windows headless build option. `BEEBJIT_HEADLESS`
  now applies to the Windows branch of `os.c`, swapping the GUI/audio
  backends for the existing null stubs. New `build_headless_win_opt.sh`
  script. Includes a small prerequisite fix to `os_time_windows.c`
  making its dependency on `log.h` explicit.

- **2026-04-27**: Add headless screen capture support. New
  `-headless-render` flag allocates the render buffer in headless mode
  (otherwise skipped as an optimisation), and a new `savescreen <f>`
  debug command writes the BGRA framebuffer to a file. Together they
  enable programmatic screen capture from automation harnesses without
  requiring GUI mode. Functional tests added asserting headless render
  output is pixel-identical to GUI rendering.

- **2026-04-29**: Add `loaddisc` debug command for runtime disc image
  mounting. `loaddisc <d> <f> [w] [m]` swaps a disc image into drive 0
  or 1 from the debugger prompt without restarting the emulator, with
  optional flags for cutting the write-protect notch (`w`) and flushing
  in-memory changes back to the host file (`m`). Pre-checks drive
  number, file readability, extension and free slot so a typo at the
  prompt does not bail the emulator. Also extracts a shared
  `disc_is_known_extension()` helper so the supported-extension list
  lives in one place (used by both `disc_create()` and the runtime
  loader). Functional tests cover both the runtime mount path and the
  non-fatal-error contract.

- **2026-04-29**: Fix `--image-base` linker flag in `build_win_opt.sh`
  and `build_win_dbg.sh`. `mingw-gcc` was treating `0x00400000` as a
  source file because of a space rather than a comma between option
  fragments. Switched to the comma form already used in
  `build_headless_win_opt.sh` so the cross-compiled Windows GUI builds
  work again.
