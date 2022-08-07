# WinDbg Cheat Sheet

> This are just important stuff that I have used.

- `$exentry` the address of the entry point.

- [How to find main](https://www.youtube.com/watch?v=FCGpXLtCzLY) when the executable doesn't have symbols.

- [Reload symbols](https://docs.microsoft.com/en-us/windows-hardware/drivers/debugger/-reload--reload-module-)

  ```sh
  .reload /f
  ```

- [Debugger Reference](https://docs.microsoft.com/en-us/windows-hardware/drivers/debugger/debugger-reference)

- YouTube reference [Examining and Modifying Registers and Memory](https://youtu.be/YwpWiPiDrDs)

  ```sh
  u <addr/symbol>                       # unassemble
  u poi(regnam+offset)                  # unassemble address at register + offset / pointer
  uf <addr/symbol>                      # unassemble function

  bp <addr/symbol>                      # breakpoint [F9]
  bp <addr/symbol> /1                   # creates a "one-shot" breakpoint. After this breakpoint is triggered, it is deleted from the breakpoint list.
  bl                                    # breakpoint lists
  bd <number>                           # breakpoint disable
  be <number>                           # breakpoint enable
  bc <number>                           # breakpoint clear

  g                                     # go/run [F5]
  .restart                              # restart

  r <regname1>, <regname2>              # viewing register/s
  r <regname1> = <value>                # modifying register/s

  db <addr> L<number>                   # displays <number> bytes starting at <address>
  dd <addr> L<number>                   # displays <number> doublewords (4 bytes) starting at <address>
  dq <addr> L<number>                   # displays <number> qwords (8 bytes) starting at <address>
  da <addr>                             # displays as ASCII string at that address until first null terminator

  eb <regname> 0x11 0x22 0x33 0x44      # modify <regname> with the 4 bytes
  ed <regname> 0xdeadbeef               # modify <regname> with the 1 doubleword (4 bytes)

  k                                     # view stack backtrace

  p / p5                                # single step over [F10] or step over 5 instructions
  t / t5                                # single step / trace into [F11 / F8] or step into 5 instructions
  gu                                    # step out [Shift + F11]
  tt                                    # trace to the next return
  pt                                    # step over to the next return
  pa <addr/symbol>                      # step to address or symbol
  ta <addr/symbol>                      # trace to address or symbol
  ```

- [Break on access](https://docs.microsoft.com/en-us/windows-hardware/drivers/debugger/ba--break-on-access-)

- YouTube reference [BreakOnAccess](https://www.youtube.com/watch?v=6X6FKklY2Qw)

  ```sh
  ba <access-type> <data-size> <address>

  # or

  ba <access-type> <data-size> <address> /1 # creates a "one-shot" breakpoint. After this breakpoint is triggered, it is deleted from the breakpoint list.

  # Where:
  #  <access-type> == w (for write-access) && r (for read/write-access)
  #  <data-size> == 1, 2, 4, or 8 (bytes)
  #  <address> == an absolute address, symbol name, or calculation of an address like "rsp+0x24"
  ```

### Updating stack view

- This can be accomplished simply by putting `rsp` into a memory window, and setting the size to `quad hex`, and adjusting the window size to only show one quadword per row.

- YouTube reference [UpdatingStackView](https://youtu.be/GlyOqapLhN8)

### Hardware vs. Software Breakpoints on Intel Hardware
-  It is generally possible to set an unlimited number of software breakpoints, because they take the form of a single byte "interrupt 3" (0xCC) instruction which the debugger writes into the instruction stream wherever it would like to break.
- There are a limited number (4) of dedicated Intel special purpose debug registers.
- These registers inform the memory management unit that it should watch for read, write, execute, or IO accesses targeted the specified addresses/IO ports.
- The `ba` instructions are actually utilizing the hardware breakpoints behind the scenes.
- Which also supports break on execute.
- The advantage of a hardware break on execute is that the memory targeted may not actually be mapped into the program's memory space yet. But if you know the address it will be at later, once it is mapped in, then you can set a breakpoint for there and the processor will alert the debugger when the breakpoint is hit.
- This is most useful when dealing with malware which has intentional dynamism to the instructions it executes, or in the kernel where different physical memory can be mapped to the same virtual memory address at different times.

- Hardware break on execute

- YouTube reference [BreakOnAccessExecute](https://youtu.be/pzN9ZCgV0rA)

  ```sh
  # A simplified form of the break on access command is for breaking on execute is:
  ba <access type> <data size> <address>

  # or

  ba <access-type> <data-size> <address> /1 # creates a "one-shot" breakpoint. After this breakpoint is triggered, it is deleted from the breakpoint list.

  # Where:
  #  <access type> == e (for execute)
  #  <data size> == 1 (WinDbg mandates that the size must be 1 if the type is "e")
  #  <address> == an absolute address, symbol name, or calculation of an address like "rsp+0x24"
  ```

### Resources

- OpenSecurityTraining2 (OST2) [Debuggers 1011: Introductory WinDbg](https://p.ost2.fyi/courses/course-v1:OpenSecurityTraining2+Dbg1011_WinDbg1+2021_v1/course/)
- [Getting started with WinDbg Part 3](http://blog.opensecurityresearch.com/2013/12/getting-started-with-windbg-part-3.html)
