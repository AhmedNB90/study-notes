# Pagers and VIM Text Editor

## Pagers

### `less` Command Usage:
- Open a file with `less`:
  ```sh
  $ less filename
  $ less /var/log/dnf.log 
  ```
- Navigation:
  - Use arrow keys (up and down) to navigate through the file.
  - To search, use `/` followed by the word you want to search for.
  - To go to the next occurrence, press `n` (search is case sensitive).
  - To ignore case sensitivity in search, pass the `-i` option.
  - To move backwards, hold the `Shift` key and press `N`.
  - To exit, press `q`.

### `more` Command Usage:
- Open a file with `more`:
  ```sh
  $ more /var/log/syslog
  ```
- Navigation:
  - Use the `Space` key to move a page at a time.
  - To exit, press `q`.

## VIM Text Editor

### Introduction
- `vim` stands for **vi improved**.
- `vi` is mode sensitive and has different modes.

### Basic Usage:
- To add text to a file:
  - Open it with `vim` and press `i` to enter insert mode.
  - Press `Escape` to go back to default mode where you can't type anything.

### Search:
- Use the `/` key and type what you want to search for (case sensitive).
- To perform a case insensitive search, type what you want, then type the escape character `\` followed by the letter `c`.

### Navigation:
- To go to a particular line number, type a colon followed by the line number (e.g., `:10`).

### Editing:
- To copy a line of text:
  - In command mode, press the `y` key twice (`yy`).
- To paste the line:
  - In command mode, use the `P` key.
- To cut a line of text:
  - Press the `d` key.

### Exiting VIM:
- To exit:
  - Go to command mode and type `q`.
  - If you have edited something and want to save, type `wq`.
  - If you want to exit without saving, type `q!`.

```
