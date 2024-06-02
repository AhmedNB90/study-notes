Here's the content formatted in Markdown for searching files in Linux:

---

## Searching for Files in Linux

### Usage
```sh
find [path/for/directory-to-search] [param]
```

### Examples
- List all files in the current directory that have been modified in the last minute:
  ```sh
  $ find -mmin 5
  ```
- List all files modified in the last 5 minutes:
  ```sh
  $ find -mmin -5
  ```
- List all files modified more than 5 minutes ago:
  ```sh
  $ find -mmin +5
  ```
- List all files modified in the last 2 days (23-hour periods):
  ```sh
  $ find -mtime 2
  ```

### Modification vs. Change Time
- **Modification**: Refers to creating or editing a file.
- **Change Time**: Refers to changes in the file's metadata (e.g., file permissions).
- To find files with change time in the last 5 minutes:
  ```sh
  $ find -cmin -5
  ```

### Other Search Options
- **Size**: Find files with a specific size.
  ```sh
  $ find -size 512k  # Find files with size 512k
  ```
- **Name**:
  ```sh
  $ find -name "f*"  # Find files with names starting with 'f'
  ```
- **Permissions**:
  ```sh
  $ find -perm 664  # Find files with exactly 664 permissions
  $ find -perm -664 # Find files with at least 664 permissions
  $ find -perm /664 # Find files with any of these permissions
  ```

### Combined Search Criteria
- Find files with a name starting with 'f' and size of 512k:
  ```sh
  $ find -name "f*" -size 512k
  ```
- Find files with a name starting with 'f' or size of 512k:
  ```sh
  $ find -name "f*" -o -size 512k
  ```

### NOT Operator
- Exclude files with names starting with 'f':
  ```sh
  $ find -not -name "f*"
  $ find \! -name "f*"  # Same as above
  ```

### File Permissions in `find` Command
- Using numeric permissions:
  ```sh
  $ find -perm 664             # Find files with exactly 664 permissions
  $ find -perm -664            # Find files with at least 664 permissions
  $ find -perm /664            # Find files with any of these permissions
  ```
- Using symbolic permissions:
  ```sh
  $ find -perm u=rw,g=rw,o=r   # Find files with permissions u=rw,g=rw,o=r
  $ find -perm -u=rw,g=rw,o=r  # Find files with at least these permissions
  $ find -perm /u=rw,g=rw,o=r  # Find files with any of these permissions
  ```

---

This Markdown file contains essential `find` command usage and examples for searching and filtering files in Linux based on various criteria.