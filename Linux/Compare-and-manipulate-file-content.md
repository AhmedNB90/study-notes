

---

## File Content Manipulation Commands

### Viewing File Content
- `cat`: Used to view the content of files.
- `tac`: Used to view the content of a file in reverse order.

### `tail` Command
- Lists the last 10 lines of a file by default:
  ```sh
  $ tail /var/log/dnf.log
  ```
- To see the last `n` number of lines:
  ```sh
  $ tail -n 20 /var/log/dnf.log  # Outputs the last 20 lines of /var/log/dnf.log
  ```
- To see logs in real-time using the `-f` flag:
  ```sh
  $ tail -f /var/log/syslog  # Continuously displays the logs in /var/log/syslog
  ```

### `head` Command
- Displays the first 10 lines of a file by default:
  ```sh
  $ head /etc/nginx/nginx.conf
  ```
- To display the first `n` lines:
  ```sh
  $ head -n 20 /etc/nginx/nginx.conf  # Displays the first 20 lines of /etc/nginx/nginx.conf
  ```

### `sed` Command (Stream Editor)
- Used to find and replace text in files. For example, consider a file called `userdata.txt` with the following content:
  ```
  ravi seattle  usa          392223543231 india
  mark toronto canada        123445678865 canada
  john newyork usa           443567898864 usa
  ravi montreal canda        456345645674 canda
  mary ottawa  canda         334566765433 canda
  ```
- To find and replace `canda` with `canada`:
  ```sh
  $ sed 's/canda/canada/g' userdata.txt
  ```
  This command will edit the output but not write to the file. The `s` stands for substitute, and the `g` is for global search (i.e., replace all occurrences). If `g` is omitted, `sed` will replace only the first occurrence.
- To edit the file in place:
  ```sh
  $ sed -i 's/canda/canada/g' userdata.txt  # The '-i' or '--in-place' flag will edit the file with the result.
  ```

### `cut` Command
- Extracts specific fields from a file. For example, to extract only the first column (names) from `userdata.txt`:
  ```sh
  $ cut -d ' ' -f 1 userdata.txt  # '-d' is the delimiter (in this case, a space), and '-f' specifies the field (column).
  ```

### `uniq` and `sort` Commands
- `uniq`: Removes duplicate lines from the output.
  ```sh
  $ uniq countries.txt
  ```
- `sort`: Sorts the output.
  ```sh
  $ sort countries.txt
  ```

### `diff` Command
- Used to see the differences between two files:
  ```sh
  $ diff file1 file2
  ```

---

