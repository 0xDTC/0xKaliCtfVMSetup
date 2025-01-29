# History Manager Created by 0xDTC

A simple Bash script to manage your shell history for privacy. It allows you to:

- Redirect history files to `/dev/null`
- Set up a daily cron job to clear history
- Clear history on system startup
- Make history files immutable for maximum privacy
- Easily roll back changes to default behavior

---

## Features

1. **Redirect Shell History**  
   - Disables saving commands to `.bash_history`, `.zsh_history`, and `.python_history` by redirecting them to `/dev/null`.

2. **Daily Automatic Clearing**  
   - Schedules a cron job at midnight (`0 0 * * *`) to clear all history files.

3. **Clear on System Startup**  
   - Adds commands to `/etc/rc.local` (if available) to truncate history files on each reboot.

4. **Immutable History Files**  
   - Uses `chattr +i` to lock history files, preventing any modifications—even by the root user (unless the attribute is removed).

5. **Rollback to Defaults**  
   - Removes all privacy settings, restores original history functionality, removes cron job, and makes files writable again.

---

## Prerequisites

- **Operating System**: Linux (Debian-based systems tested, but should work on most Linux distros).
- **Bash Shell** (or Zsh, partially covered).
- **sudo** privileges:
  - Required to modify `/etc/rc.local` and apply `chattr +i` or `chattr -i` on history files.

---

## Installation

1. **Download** or **copy** the `history_manager.sh` script into a directory of your choice.
2. Make it executable:
```bash
chmod +x Clean-History
```

---

## Usage

1. **Run the Script**:
```bash
./Clean-History
```

2. **Choose an Option**:

   | Option | Description                                                                 |
   |-------:|:----------------------------------------------------------------------------|
   | **1**  | Redirect history files to `/dev/null`                                       |
   | **2**  | Set up a daily cron job to clear history at `0 0 * * *`                     |
   | **3**  | Clear history on system startup (via `/etc/rc.local`)                       |
   | **4**  | Make history files immutable                                                |
   | **5**  | Restore history logging to default (removes redirect to `/dev/null`)        |
   | **6**  | Remove daily cron job                                                       |
   | **7**  | Remove startup clearing commands from `/etc/rc.local`                       |
   | **8**  | Make history files writable again                                           |
   | **9**  | **Enable all privacy settings** (1–4 combined)                              |
   | **10** | **Rollback to default** (5–8 combined)                                      |
   | **11** | Exit the script                                                             |

---

## Examples

- **Enable All Privacy Settings**  
  Select `9` from the menu. This will:
  1. Redirect history files to `/dev/null`.
  2. Create a cron job to clear history at midnight.
  3. Add commands to `/etc/rc.local` to clear history on startup.
  4. Make history files immutable (`chattr +i`).

- **Rollback to Default Settings**  
  Select `10` from the menu. This will:
  1. Restore history logging for Bash, Zsh, and Python.
  2. Remove the nightly cron job.
  3. Remove startup commands in `/etc/rc.local`.
  4. Make history files writable again (`chattr -i`).

---

## Important Notes

- **Making Files Immutable**:  
  When you use `chattr +i` on a file, it cannot be modified or deleted—even by `root`—until you remove the attribute (`chattr -i`).
- **Shell Session Behavior**:  
  Redirecting to `/dev/null` only affects **future** command logging. Existing lines in history files remain until you clear or truncate them.
- **In-Memory History**:  
  Your current session’s commands might still appear when running the `history` command until the shell terminates. They won't persist after you exit.
- **Python REPL**:  
  If you launch a Python interpreter, it typically writes to `~/.python_history`. The script optionally redirects this as well.

---

## Troubleshooting

1. **`sed: extra characters after command`**  
   - This script uses a pipe (`|`) delimiter for the `sed` commands to handle special symbols like `>`. If you see any `sed`-related errors, ensure your system’s `sed` version supports this usage.
2. **Permissions Errors**  
   - Make sure you run this script **in a user shell that has `sudo` privileges**. The script will prompt for a password if needed to modify `/etc/rc.local` or apply `chattr`.

---

## Contributing

1. Fork this repository.
2. Create a new branch: `git checkout -b feature/your-feature`.
3. Commit your changes: `git commit -m 'Add some feature'`.
4. Push to the branch: `git push origin feature/your-feature`.
5. Open a Pull Request.
