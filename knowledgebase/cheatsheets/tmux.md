# tmux Cheat Sheet

A comprehensive guide to `tmux`, a terminal multiplexer that allows you to manage multiple terminal sessions within a single window. Ideal for managing long-running processes, organizing workspaces, and enhancing productivity.

---

## Table of Contents

1. [Installation](#installation)
2. [Starting tmux](#starting-tmux)
3. [Basic Commands](#basic-commands)
4. [Session Management](#session-management)
5. [Window Management](#window-management)
6. [Pane Management](#pane-management)
7. [Customization](#customization)
8. [Useful Tips & Tricks](#useful-tips--tricks)
9. [Common Use Cases](#common-use-cases)

---

## Installation

Ensure `tmux` is installed on your Ubuntu WSL environment.

```bash
sudo apt update
sudo apt install tmux
```

Verify installation:

```bash
tmux -V
# Example Output: tmux 3.2a
```

---

## Starting tmux

### Start a New Session

```bash
tmux
```

### Start a New Session with a Name

```bash
tmux new -s mysession
```

### Attach to an Existing Session

```bash
tmux attach -t mysession
# or
tmux a -t mysession
```

### List All Sessions

```bash
tmux ls
```

### Detach from Current Session

- **Keyboard Shortcut:** `Ctrl + b` then `d`

---

## Basic Commands

**tmux** uses a **prefix key** (`Ctrl + b` by default) followed by a command key.

- **Prefix Key:** `Ctrl + b`

### Common Prefix Commands

| Action                          | Command                        |
|---------------------------------|--------------------------------|
| Send prefix key to application  | `Ctrl + b` then `Ctrl + b`      |
| List key bindings               | `Ctrl + b` then `?`             |
| Reload configuration            | `Ctrl + b` then `:` then type `source-file ~/.tmux.conf` |

---

## Session Management

### Create a New Session

```bash
tmux new -s mysession
```

### List Sessions

```bash
tmux ls
```

### Attach to a Session

```bash
tmux attach -t mysession
```

### Detach from Session

- **Keyboard Shortcut:** `Ctrl + b` then `d`

### Kill a Session

```bash
tmux kill-session -t mysession
```

### Rename a Session

- **Command:** `tmux rename-session -t oldname newname`

---

## Window Management

Each session can have multiple windows, akin to tabs.

### Create a New Window

- **Keyboard Shortcut:** `Ctrl + b` then `c`

### Switch to Next Window

- **Keyboard Shortcut:** `Ctrl + b` then `n`

### Switch to Previous Window

- **Keyboard Shortcut:** `Ctrl + b` then `p`

### Select Window by Number

- **Keyboard Shortcut:** `Ctrl + b` then `<window-number>`
  
  *Example:* `Ctrl + b` then `2` to switch to window 2.

### Rename Current Window

- **Keyboard Shortcut:** `Ctrl + b` then `,`

### Close Current Window

- **Keyboard Shortcut:** `Ctrl + b` then `&`

---

## Pane Management

Split windows into multiple panes for parallel tasks.

### Split Horizontally

- **Keyboard Shortcut:** `Ctrl + b` then `"` 

### Split Vertically

- **Keyboard Shortcut:** `Ctrl + b` then `%`

### Navigate Between Panes

- **Keyboard Shortcut:** `Ctrl + b` then arrow keys (`←`, `→`, `↑`, `↓`)

### Resize Panes

- **Keyboard Shortcut:** `Ctrl + b` then `:` and type `resize-pane -D` / `-U` / `-L` / `-R`
  
  *Alternatively,* use `Ctrl + b` then `Ctrl + arrow keys` (if configured).

### Close Current Pane

- **Keyboard Shortcut:** `Ctrl + b` then `x`

### Swap Panes

- **Keyboard Shortcut:** `Ctrl + b` then `o`

---

## Customization

Customize `tmux` behavior via the `~/.tmux.conf` file.

### Common Configurations

```bash
# Set prefix to Ctrl + a (optional)
# set-option -g prefix C-a
# unbind default prefix
# unbind C-b
# bind new prefix
# bind C-a send-prefix

# Enable mouse support
set -g mouse on

# Set window splitting shortcuts
bind | split-window -h
bind - split-window -v

# Reload config with prefix + r
bind r source-file ~/.tmux.conf \; display-message "Config Reloaded!"

# Enable vim-style pane switching
bind h select-pane -L
bind j select-pane -D
bind k select-pane -U
bind l select-pane -R

# Set status bar
set -g status-bg colour235
set -g status-fg colour136
set -g status-left '[: #S | #I:#W]'
```

### Applying Configuration

After editing `~/.tmux.conf`, reload it:

```bash
tmux source-file ~/.tmux.conf
```

---

## Useful Tips & Tricks

- **Copy Mode:** Enter copy mode to navigate and copy text.
  - **Keyboard Shortcut:** `Ctrl + b` then `[`
  - **Exit Copy Mode:** `q` or `Esc`

- **Search in Copy Mode:**
  - Press `/` and type the search query.

- **Scroll Up/Down:**
  - Use arrow keys or `PgUp`/`PgDn` in copy mode.

- **Synchronize Panes:**
  - Execute the same command in all panes.
  - **Command:** `Ctrl + b` then `:setw synchronize-panes on`

- **Lock tmux Session:**
  - **Command:** `Ctrl + b` then `:lock-server`

- **Display Current Key Bindings:**
  - **Keyboard Shortcut:** `Ctrl + b` then `?`

---

## Common Use Cases

### Running Multiple Processes

Organize different processes in separate panes or windows.

```bash
# Window 1: Development server
tmux new -s dev
# Inside tmux
# Pane 1: Run server
python manage.py runserver

# Pane 2: Monitor logs
tail -f logs/server.log
```

### Remote Workflows

Maintain persistent sessions across SSH connections.

```bash
# On remote server
tmux new -s remote
# Start long-running task
python train_model.py

# Detach and disconnect
Ctrl + b then d

# Reattach later
tmux attach -t remote
```

### Managing Git Operations

Use separate panes for editing, committing, and viewing status.

```bash
# Split window vertically
Ctrl + b then %

# Pane 1: Edit code
vim main.py

# Pane 2: Git status and commits
git status
git commit -m "Update main.py"
```

### Monitoring System Resources

Run monitoring tools alongside development.

```bash
# Split window horizontally
Ctrl + b then "

# Pane 1: Develop
vim script.py

# Pane 2: Monitor
htop
```

---

## Example Workflow

1. **Start a New Session:**

    ```bash
    tmux new -s ai_project
    ```

2. **Create Windows:**

    - **Window 1:** Code Editor
        - **Keyboard Shortcut:** `Ctrl + b` then `c`
        - **Commands:**
            ```bash
            vim main.py
            ```

    - **Window 2:** Terminal
        - **Keyboard Shortcut:** `Ctrl + b` then `c`
        - **Commands:**
            ```bash
            python main.py
            ```

    - **Window 3:** Logs
        - **Keyboard Shortcut:** `Ctrl + b` then `c`
        - **Commands:**
            ```bash
            tail -f logs/output.log
            ```

3. **Navigate Between Windows:**

    - **Next Window:** `Ctrl + b` then `n`
    - **Previous Window:** `Ctrl + b` then `p`
    - **Select Window by Number:** `Ctrl + b` then `1`, `2`, `3`, etc.

4. **Detach and Reattach:**

    - **Detach:** `Ctrl + b` then `d`
    - **Reattach:** `tmux attach -t ai_project`

---

## References

- [tmux Official Documentation](https://github.com/tmux/tmux/wiki)
- [tmux Cheat Sheet & Quick Reference](https://tmuxcheatsheet.com/)
- [Mastering tmux](https://pragprog.com/titles/bhtmux/mastering-tmux/)