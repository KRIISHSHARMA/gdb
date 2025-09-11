# GDB Debugging Guide

## Run GDB

```bash
gdb vmlinux
```

> \[!NOTE]
> Use `vmlinux` when debugging a Linux kernel (or kernel module) with symbols included.

To attach to a remote or VM instance using GDB:

```gdb
target remote :<port>
```

> \[!TIP]
> Use this to connect to a running kernel or QEMU-based VM exposed via GDB stub (e.g., `qemu -s -S`).

---

## Starting UE in GDB

```bash
sudo gdb --args sudo NFAPI_TRACE_LEVEL=debug ./nr-uesoftmodem \
  -r 106 --numerology 1 --band 78 -C 3619200000 --sa \
  --uicc0.imsi 001010000000001 --rfsim
```

---

## Inside GDB

### Load Object File (Optional if using `--args`)

```gdb
file ./nr-uesoftmodem
```

> \[!NOTE]
> Useful if you didn't pass the binary using `--args` when launching GDB.

---

### Run the Program

```gdb
run
```

---

## When a Breakpoint is Hit

### Core Inspection Commands

```gdb
bt                # Show backtrace (call stack)
info args         # Show function arguments
info local        # Show local variables
info registers    # Show register contents
info threads      # List all threads
thread <number>   # Switch to a specific thread
```

> \[!TIP]
> Use `info threads` to get a list of active threads, then switch using `thread <id>`.

---

### Source and Assembly Views

```gdb
list              # Show C source code where the program stopped
disassemble       # Show the assembly around the current instruction pointer
```

---

### Stepping Through Code

```gdb
next              # Step over the current line
step              # Step into the current function call
finish            # Run until the current function returns
```

> \[!NOTE]
> `step` enters a function, while `next` skips over it. Use `finish` to complete a function call and return to its caller.

---

## TUI (Text User Interface) Mode

Enable TUI within GDB:

```gdb
tui enable
```

Or launch GDB directly in TUI mode:

```bash
gdb -tui --args ...
```

> \[!TIP]
> Inside GDB, toggle TUI mode using: `Ctrl + x` then `a`

---

## Advanced GDB Features

### Conditional Breakpoints

```gdb
break <file>:<line> if <condition>
# Example:
break main.c:42 if i > 10
```

---

### Watchpoints

```gdb
watch <var>       # Break when variable is written to
rwatch <var>      # Break when variable is read
awatch <var>      # Break on read or write
```

---

### Setting Breakpoints on Functions

```gdb
break <function_name>
```

---

### Inspecting Memory

```gdb
x/<n><format> <address>
# Examples:
x/16xw $sp     # 16 words in hex from stack pointer
x/s <addr>     # Display string at address
```

---

### Modifying Variables

```gdb
set variable <var> = <value>
# Example:
set variable retry_count = 0
```

---

### Logging GDB Output

```gdb
set logging on
set logging file gdb_log.txt  # optional
```

---

### Customizing with Hooks and Scripts

Example: Automatically print backtrace and locals after every stop

```gdb
define hook-stop
  bt
  info local
end
```

Save it in your `.gdbinit` file for persistent use.

---
