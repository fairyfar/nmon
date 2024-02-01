[中文](README-中文.md) | [ENGLISH](README.md)

This project is used to fix bugs and enhance features for `nmon`.

# 1.Bug Fixes

## 1.1. BUG1: Main process initialization synchronization when `nmon` works in `loging (not cursed) mode`.

### Problem

When `nmon` works in `loging mode` (for example using the parameter `-f` or `-F`), the main process returns before the child process has completed initialization.

In some cases, for example in our docker test environment, the following command takes 20 seconds to return.

```
linux_bbbp("netstat -r", "/bin/netstat -r 2>/dev/null", WARNING);
```

So, there is no sampled data during this period, and it is difficult for callers to know the actual start time of sampling.

### Solution

Set up a semaphore, then `post` semaphore after the child process is initialized. The main process returns and prints the `PID` after receiving the semaphore.

### Fixing Log

Already fixed.

# 2. Compile

See the `makefile` file for details.

Many kinds of target platforms are predefined in `makefile`. Please modify `makefile` first to keep only the target platform definitions and delete other definitions.

For example, if you need to compile the `Red Hat 7` platform program, the `makefile` is as follows:

```makefile
CFLAGS=-g -O3 -Wall
LDFLAGS=-lncurses -lm -lpthread
FILE=lmon16n.c

nmon_x86_rhel7: $(FILE)
	cc -o nmon_x86_rhel7 $(FILE) $(CFLAGS) $(LDFLAGS) -D X86 -D RHEL7
```

Then, do the `make` command.

# 3. Original Project Informations

Official website for original `nmon` project: [http://nmon.sourceforge.net](http://nmon.sourceforge.net)

# 4. Open Source License

GNU General Public License version 3.0 (GPLv3)

Scripted by FairyFar. [www.200yi.com](http://www.200yi.com/)
