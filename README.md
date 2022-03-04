This project is used to fix bugs and enhance features for `nmon`.

# 1.Bug Fixes

## 1.1. BUG1: Main process initialization synchronization when `nmon` works in `ralfmode`.

### Problem

When `nmon` works in `ralfmode` (using the parameter `-p`), the main process returns before the child process has completed initialization.

In some cases, for example in our docker test environment, the following command takes 20 seconds to return.

```
linux_bbbp("netstat -r", "/bin/netstat -r 2>/dev/null", WARNING);
```

So, there is no sampled data during this period, and it is difficult for callers to know the actual start time of sampling.

### Solution

Set up a semaphore, then `post` semaphore after the child process is initialized. The main process returns and prints the `PID` after receiving the semaphore.

### Fixing Log

Already fixed.

# 2. Original Project Informations

Official website for original `nmon` project: [http://nmon.sourceforge.net](http://nmon.sourceforge.net)

# 3. Open Source License

GNU General Public License version 3.0 (GPLv3)

Scripted by FairyFar. [www.200yi.com](http://www.200yi.com/)
