# Helix TreeView File Navigation Guide

Add a sidebar file explorer/treeview navigation to the [Helix Editor](https://helix-editor.com/) using **Yazi** and **Tmux**.

---

![Demo](Demo.mp4)

---

## 📋 Prerequisites & Requirements

Before setting up, make sure you have the following installed and configured:

1. **Tmux**: Required for running Helix and Yazi in adjacent panes.
2. **Yazi**: A fast terminal file manager written in Rust.
   * [Yazi Installation Guide](https://yazi-rs.github.io/docs/installation)
3. **TreeView Script**:
   * Download the script from the repository: [TreeView-for-helix](https://github.com/MicroDevX/TreeView-for-helix)
   * Make it executable and place it in your `$PATH`:
     ```bash
     chmod +x treeview
     mv treeview /usr/local/bin/  # Or any directory in your $PATH
     ```

---

## ⚙️ Configuration Files

Configure **Yazi** to strip down its layout and enable sending file-open commands directly to your Helix Tmux pane.

### 1. File Manager Layout (`~/.config/yazi/yazi.toml`)

Update the manager layout ratio to single-column display:

```toml
[mgr]
ratio = [ 0, 1, 0 ]
```

### 2. Custom Keymaps (`~/.config/yazi/keymap.toml`)

Map the `<Enter>` key in Yazi so that pressing it on a file sends an `:open <filepath>` command to the Helix pane on the right:

```toml
[[mgr.prepend_keymap]]
on   = [ "<Enter>" ]
run  = '''
shell '
  if [ -d %h ]; then
    yazi --remote "open"
  else
    tmux send-keys -t :.right Escape Escape ":open %h" Enter
  fi
'
'''
desc = "Enter directory or open file in Helix"
```

---

## 🚀 How to Use

1. Open your project root directory in your terminal.
2. Ensure you are inside a **Tmux** session.
3. Run the `treeview` command in your terminal:
   ```bash
   treeview
   ```

The script will set up adjacent Tmux panes featuring the Yazi sidebar on the left and Helix on the right. Pressing `<Enter>` on any file inside Yazi will open it instantly in Helix!
