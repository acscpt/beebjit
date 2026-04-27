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
