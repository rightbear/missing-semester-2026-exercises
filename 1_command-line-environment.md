# Exercises

All classes in this course are accompanied by a series of exercises. Some give you a specific task to do, while others are open-ended, like "try using X and Y programs". We highly encourage you to try them out.

We have not written solutions for the exercises. If you are stuck on anything in particular, feel free to post in `#missing-semester-forum` on [Discord](https://ossu.dev/#community) or send us an email describing what you've tried so far, and we will try to help you out. These exercises will also likely work well as initial prompts in a conversation with an LLM where you can interactively dive into the topic. The real value in these exercises is the journey of discovering the answers, not the answer itself. We encourage you to follow tangents and ask "why" as you work through them, rather than just looking for the shortest path to the solution.

1. For this course, you need to be using a Unix shell like Bash or ZSH. If you are on Linux or macOS, you don't have to do anything special. If you are on Windows, you need to make sure you are not running cmd.exe or PowerShell; you can use [Windows Subsystem for Linux](https://docs.microsoft.com/en-us/windows/wsl/) or a Linux virtual machine to use Unix-style command-line tools. To make sure you're running an appropriate shell, you can try the command `echo $SHELL`. If it says something like `/bin/bash` or `/usr/bin/zsh`, that means you're running the right program.

    ## **Answer** 
    ```
    rightbear@Rightbear:~$ echo "This is a WSL environment"
    This is a WSL environment
    rightbear@Rightbear:~$ echo $SHELL
    /bin/bash
    ```

2. What does the `-l` flag to `ls` do? Run `ls -l /` and examine the output.
   What do the first 10 characters of each line mean? (Hint: `man ls`)

    ## **Answer** 
    ### Demo
    ```
    rightbear@Rightbear:~$ man ls
    …
           -l     use a long listing format
    …
    rightbear@Rightbear:~$ ls -l /
    total 2796
    lrwxrwxrwx   1 root root       7 Apr 22  2024 bin -> usr/bin
    drwxr-xr-x   2 root root    4096 Feb 26  2024 bin.usr-is-merged
    drwxr-xr-x   2 root root    4096 Apr 22  2024 boot
    drwxr-xr-x  15 root root    3860 Apr 15 13:37 dev
    drwxr-xr-x  92 root root    4096 Apr 15 13:36 etc
    drwxr-xr-x   3 root root    4096 Mar 19 16:46 home
    -rwxr-xr-x   1 root root 2781568 Dec 12 09:58 init
    lrwxrwxrwx   1 root root       7 Apr 22  2024 lib -> usr/lib
    drwxr-xr-x   2 root root    4096 Apr  8  2024 lib.usr-is-merged
    lrwxrwxrwx   1 root root       9 Apr 22  2024 lib64 -> usr/lib64
    drwx------   2 root root   16384 Mar 19 16:45 lost+found
    drwxr-xr-x   2 root root    4096 Feb 10 08:54 media
    drwxr-xr-x   5 root root    4096 Mar 19 16:45 mnt
    drwxr-xr-x   3 root root    4096 Apr  7 10:51 opt
    dr-xr-xr-x 270 root root       0 Apr 15 13:36 proc
    drwx------   3 root root    4096 Apr  9 17:35 root
    drwxr-xr-x  21 root root     640 Apr 15 13:37 run
    lrwxrwxrwx   1 root root       8 Apr 22  2024 sbin -> usr/sbin
    drwxr-xr-x   2 root root    4096 Mar 31  2024 sbin.usr-is-merged
    drwxr-xr-x   2 root root    4096 Mar 19 16:45 snap
    drwxr-xr-x   2 root root    4096 Feb 10 08:54 srv
    dr-xr-xr-x  13 root root       0 Apr 15 13:36 sys
    drwxrwxrwt   9 root root    4096 Apr 15 13:37 tmp
    drwxr-xr-x  12 root root    4096 Feb 10 08:54 usr
    drwxr-xr-x  13 root root    4096 Mar 19 16:45 var
    ```
    ### Explain
    The first 10 characters of an “ls -l” output represent the file type and access permissions. The first character means different file types (regular file, directory, symbolic link, and so on). The next 9 characters mean permissions (read, write and execute), and they are divided into three groups of three characters each (owner, groups and others).

3. In the command `find ~/Downloads -type f -name "*.zip" -mtime +30`, the
   `*.zip` is a "glob". What is a glob? Create a test directory with some
   files and experiment with patterns like `ls *.txt`, `ls file?.txt`, and
   `ls {a,b,c}.txt`. See [Pattern
   Matching](https://www.gnu.org/software/bash/manual/html_node/Pattern-Matching.html)
   in the Bash manual.

4. What's the difference between `'single quotes'`, `"double quotes"`, and
   `$'ANSI quotes'`? Write a command that echoes a string containing a
   literal `$`, a `!`, and a newline character. See
   [Quoting](https://www.gnu.org/software/bash/manual/html_node/Quoting.html).

5. The shell has three standard streams: stdin (0), stdout (1), and stderr
   (2). Run `ls /nonexistent /tmp` and redirect stdout to one file and
   stderr to another. How would you redirect both to the same file? See
   [Redirections](https://www.gnu.org/software/bash/manual/html_node/Redirections.html).

6. `$?` holds the exit status of the last command (0 = success). `&&` runs
   the next command only if the previous succeeded; `||` runs it only if
   the previous failed. Write a one-liner that creates `/tmp/mydir` only if
   it doesn't already exist. See [Exit
   Status](https://www.gnu.org/software/bash/manual/html_node/Exit-Status.html).

7. Why does `cd` have to be built into the shell itself rather than a
   standalone program? (Hint: think about what a child process can and
   cannot affect in its parent.)

8. Write a script that takes a filename as an argument (`$1`) and checks
   whether the file exists using `test -f` or `[ -f ... ]`. It should print
   different messages depending on whether the file exists. See [Bash
   Conditional
   Expressions](https://www.gnu.org/software/bash/manual/html_node/Bash-Conditional-Expressions.html).

9. Save the script from the previous exercise to a file (e.g., `check.sh`).
   Try running it with `./check.sh somefile`. What happens? Now run
   `chmod +x check.sh` and try again. Why is this step necessary? (Hint:
   look at `ls -l check.sh` before and after the `chmod`.)

10. What happens if you add `-x` to the `set` flags in a script? Try it with
    a simple script and observe the output. See [The Set
    Builtin](https://www.gnu.org/software/bash/manual/html_node/The-Set-Builtin.html).

11. Write a command that copies a file to a backup with today's date in the
    filename (e.g., `notes.txt` → `notes_2026-01-12.txt`). (Hint: `$(date
    +%Y-%m-%d)`). See [Command
    Substitution](https://www.gnu.org/software/bash/manual/html_node/Command-Substitution.html).

12. Modify the flaky test script from the lecture to accept the test command
    as an argument instead of hardcoding `cargo test my_test`. (Hint: `$1`
    or `$@`). See [Special
    Parameters](https://www.gnu.org/software/bash/manual/html_node/Special-Parameters.html).

13. Use pipes to find the 5 most common file extensions in your home
    directory. (Hint: combine `find`, `grep` or `sed` or `awk`, `sort`,
    `uniq -c`, and `head`.)

14. `xargs` converts lines from stdin into command arguments. Use `find` and
    `xargs` together (not `find -exec`) to find all `.sh` files in a
    directory and count the lines in each with `wc -l`. Bonus: make it
    handle filenames with spaces. (Hint: `-print0` and `-0`). See `man
    xargs`.

15. Use `curl` to fetch the HTML of the course website
    (`https://missing.csail.mit.edu/`) and pipe it to `grep` to count how
    many lectures are listed. (Hint: look for a pattern that appears once
    per lecture; use `curl -s` to silence the progress output.)

16. [`jq`](https://jqlang.github.io/jq/) is a powerful tool for processing
    JSON data. Fetch the sample data at
    `https://microsoftedge.github.io/Demos/json-dummy-data/64KB.json` with
    `curl` and use `jq` to extract just the names of people whose version
    is greater than 6. (Hint: pipe to `jq .` first to see the structure;
    then try `jq '.[] | select(...) | .name'`)

17. `awk` can filter lines based on column values and manipulate output.
    For example, `awk '$3 ~ /pattern/ {$4=""; print}'` prints only lines
    where the third column matches `pattern`, while omitting the fourth
    column. Write an `awk` command that prints only lines where the second
    column is greater than 100, and swaps the first and third columns. Test
    with: `printf 'a 50 x\nb 150 y\nc 200 z\n'`

18. Dissect the SSH log pipeline from the lecture: what does each step do?
    Then build something similar to find your most-used shell commands from
    `~/.bash_history` (or `~/.zsh_history`).