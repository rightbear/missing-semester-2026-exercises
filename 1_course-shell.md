# Exercises

All classes in this course are accompanied by a series of exercises. Some give you a specific task to do, while others are open-ended, like "try using X and Y programs". We highly encourage you to try them out.

We have not written solutions for the exercises. If you are stuck on anything in particular, feel free to post in `#missing-semester-forum` on [Discord](https://ossu.dev/#community) or send us an email describing what you've tried so far, and we will try to help you out. These exercises will also likely work well as initial prompts in a conversation with an LLM where you can interactively dive into the topic. The real value in these exercises is the journey of discovering the answers, not the answer itself. We encourage you to follow tangents and ask "why" as you work through them, rather than just looking for the shortest path to the solution.

1. For this course, you need to be using a Unix shell like Bash or ZSH. If you are on Linux or macOS, you don't have to do anything special. If you are on Windows, you need to make sure you are not running cmd.exe or PowerShell; you can use [Windows Subsystem for Linux](https://docs.microsoft.com/en-us/windows/wsl/) or a Linux virtual machine to use Unix-style command-line tools. To make sure you're running an appropriate shell, you can try the command `echo $SHELL`. If it says something like `/bin/bash` or `/usr/bin/zsh`, that means you're running the right program.

    ## **Answer**
    ### Demo
    ```console
    rightbear@Rightbear:~$ echo "This is a WSL environment"
    This is a WSL environment
    rightbear@Rightbear:~$ echo $SHELL
    /bin/bash
    ```

2. What does the `-l` flag to `ls` do? Run `ls -l /` and examine the output. What do the first 10 characters of each line mean? (Hint: `man ls`)

    ## **Answer** 
    ### Demo
    ```console
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
    ### Explanation
    The first 10 characters of an `ls -l` output represent the file type and access permissions. The first character means different file types (regular file, directory, symbolic link, and so on). The next 9 characters mean permissions (read, write and execute), and they are divided into three groups of three characters each (owner, groups and others).

3. In the command `find ~/Downloads -type f -name "*.zip" -mtime +30`, the `*.zip` is a "glob". What is a glob? Create a test directory with some files and experiment with patterns like `ls *.txt`, `ls file?.txt`, and `ls {a,b,c}.txt`. See [Pattern Matching](https://www.gnu.org/software/bash/manual/html_node/Pattern-Matching.html) in the Bash manual.

    ## **Answer** 
    ### Demo
    ```console
    rightbear@Rightbear:~$ ls *.txt
    a.txt  b.txt  c.txt  err.txt  files.txt  info.txt  notes.txt  numbers.txt  result.txt  result2.txt
    rightbear@Rightbear:~$ ls file?.txt
    files.txt
    rightbear@Rightbear:~$ ls {a,b,c}.txt
    a.txt  b.txt  c.txt
    rightbear@Rightbear:~$
    ```

4. What's the difference between `'single quotes'`, `"double quotes"`, and `$'ANSI quotes'`? Write a command that echoes a string containing a literal `$`, a `!`, and a newline character. See [Quoting](https://www.gnu.org/software/bash/manual/html_node/Quoting.html).

    ## **Answer** 
    ### Explanation
    * Single Quotes (`'...'`): Enclosing characters in single quotes preserves the literal value of each character within the quotes. A single quote may not occur between single quotes, even when preceded by a backslash (`\`) .
    * Double quotes (`"..."`): Enclosing characters in double quotes preserves the literal value of all characters within the quotes. The backslash (`\`) retains special meaning only when followed by `$`, `'`, `"`, `\`, or a newline(`\n`).
    * ANSI Quotes (`$'...'`): Character sequences of the form $'string' are treated as a special kind of single quotes. The sequence expands to string, with backslash-escaped characters in string replaced as specified by the ANSI C standard.

    ### Demo
    ```console
    rightbear@Rightbear:~$ echo $'Here is a dollar sign $, a exclamation mark !, and new line symbol \n'
    Here is a dollar sign $, a exclamation mark !, and new line symbol

    rightbear@Rightbear:~$
    ```

5. The shell has three standard streams: stdin (0), stdout (1), and stderr (2). Run `ls /nonexistent /tmp` and redirect stdout to one file and stderr to another. How would you redirect both to the same file? See [Redirections](https://www.gnu.org/software/bash/manual/html_node/Redirections.html).

    ## **Answer** 
    ### Demo
    ```console
    rightbear@Rightbear:~$ ls /tmp
    snap-private-tmp
    systemd-private-2da47142d2e64a0186a68d9314f8246b-systemd-logind.service-hv8plT
    systemd-private-2da47142d2e64a0186a68d9314f8246b-systemd-resolved.service-wwZZ9C
    systemd-private-2da47142d2e64a0186a68d9314f8246b-systemd-timesyncd.service-TyjTcA
    systemd-private-2da47142d2e64a0186a68d9314f8246b-wsl-pro.service-uWTZTp
    rightbear@Rightbear:~$ ls /nonexistent
    ls: cannot access '/nonexistent': No such file or directory
    rightbear@Rightbear:~$ ls /nonexistent /tmp >practice5.txt 2>&1
    rightbear@Rightbear:~$ cat practice5.txt
    ls: cannot access '/nonexistent': No such file or directory
    /tmp:
    snap-private-tmp
    systemd-private-2da47142d2e64a0186a68d9314f8246b-systemd-logind.service-hv8plT
    systemd-private-2da47142d2e64a0186a68d9314f8246b-systemd-resolved.service-wwZZ9C
    systemd-private-2da47142d2e64a0186a68d9314f8246b-systemd-timesyncd.service-TyjTcA
    systemd-private-2da47142d2e64a0186a68d9314f8246b-wsl-pro.service-uWTZTp
    ```

6. `$?` holds the exit status of the last command (0 = success). `&&` runs the next command only if the previous succeeded; `||` runs it only if the previous failed. Write a one-liner that creates `/tmp/mydir` only if it doesn't already exist. See [Exit Status](https://www.gnu.org/software/bash/manual/html_node/Exit-Status.html).

    ## **Answer** 
    ### Option1 Demo
    ```console
    rightbear@Rightbear:~$ ls /tmp/mydir
    ls: cannot access '/tmp/mydir': No such file or directory
    rightbear@Rightbear:~$ [ -d /tmp/mydir ] || mkdir /tmp/mydir && touch /tmp/mydir/newfile.txt
    rightbear@Rightbear:~$ ls /tmp/mydir
    newfile.txt
    ```

    ### Option2 Demo
    ```console
    rightbear@Rightbear:~$ ls /tmp/mydir
    ls: cannot access '/tmp/mydir': No such file or directory
    rightbear@Rightbear:~$ mkdir -p /tmp/mydir && touch /tmp/mydir/newfile.txt

    ```

7. Why does `cd` have to be built into the shell itself rather than a standalone program? (Hint: think about what a child process can and cannot affect in its parent.)
   
    ## **Answer** 
    ### Explanation
    Changing the shell’s current working directory must affect the shell process itself. Standalone programs, like executables, run as separate child processes, so any directory change they perform would apply only to that child and not persist when it exits. Making `cd` a shell builtin allows the shell to modify its own process state.

8. Write a script that takes a filename as an argument (`$1`) and checks whether the file exists using `test -f` or `[ -f ... ]`. It should print different messages depending on whether the file exists. See [Bash Conditional Expressions](https://www.gnu.org/software/bash/manual/html_node/Bash-Conditional-Expressions.html).

    ## **Answer** 
    ### Script (check.sh)
    ```bash
    #!/bin/bash
    set -eo pipefail

    # check if the argument exists
    if [ -z "$1" ]; then
        echo "Usage: $0 {filename}"
        exit 1
    fi

    # check if the filename passed as the argument exists in currrent path
    if [ -f "$1" ]; then
            echo "Congratulations! The passed file exists!";
    else
            echo "Wake up! You don't have the passed file";
    fi;
    ```

    ### Demo
    ```console
    rightbear@Rightbear:~$ ./check.sh exist.txt
    Congratulations! The passed file exists!
    rightbear@Rightbear:~$ ./check.sh nonexist.txt
    Wake up! You don't have the passed file
    ```

9. Save the script from the previous exercise to a file (e.g., `check.sh`). Try running it with `./check.sh somefile`. What happens? Now run `chmod +x check.sh` and try again. Why is this step necessary? (Hint: look at `ls -l check.sh` before and after the `chmod`.)

    ## **Answer** 
    ### Explanation
    In Linux, security is built on the principle that files are not executable by default. This prevents users (or malicious scripts) from accidentally running a data file or a document as if it were a program, which could cause system instability. When you type `./check.sh`, you are telling the shell: "Find this file and execute the instructions inside it." The kernel checks the file's metadata for that little "**x**" bit. If it's missing, the kernel refuses to load the file into memory as a process.
    The `chmod` command changes the permissions of a file or directory to all types of users.The `+x` option specifies that executable permissions should be added. The `+` indicates addition, and `x` represents the executable permission. In summary, the command `chmod +x` is used to add executable permissions to a file in Linux.

    ### Demo
    ```console
    rightbear@Rightbear:~$ ./check.sh exist.txt
    -bash: ./check.sh: Permission denied
    rightbear@Rightbear:~$ ls -l check.sh
    -rw-r--r-- 1 ejhcaer ejhcaer 342 Apr 16 14:30 check.sh
    rightbear@Rightbear:~$ ./check.sh exist.txt
    -bash: ./check.sh: Permission denied
    rightbear@Rightbear:~$ chmod +x check.sh
    rightbear@Rightbear:~$ ls -l check.sh
    -rwxr-xr-x 1 ejhcaer ejhcaer 342 Apr 16 14:30 check.sh
    rightbear@Rightbear:~$ ./check.sh exist.txt
    Congratulations! The passed file exists!
    ```

10. What happens if you add `-x` to the `set` flags in a script? Try it with
    a simple script and observe the output. See [The Set Builtin](https://www.gnu.org/software/bash/manual/html_node/The-Set-Builtin.html).

    ## **Answer** 
    ### Explanation
    `set -x` will print commands and their arguments as they are executed.

    ### Script (testset.sh)
    ```bash
    #!/bin/bash

    set -x

    FILE="*.txt"
    ls $FILE
    ```

    ### Demo
    ```console
    rightbear@Rightbear:~$ ./testset.sh
    + FILE='*.txt'
    + ls a.txt b.txt c.txt err.txt exist.txt files.txt info.txt notes.txt numbers.txt result.txt result2.txt
    a.txt  c.txt    exist.txt  info.txt   numbers.txt    result.txt
    b.txt  err.txt  files.txt  notes.txt  result2.txt
    ```

11. Write a command that copies a file to a backup with today's date in the filename (e.g., `notes.txt` → `notes_2026-01-12.txt`). (Hint: `$(date+%Y-%m-%d)`). See [Command Substitution](https://www.gnu.org/software/bash/manual/html_node/Command-Substitution.html).

    ## **Answer** 

    ### Original Script (flasky.sh)
    ```bash
    #!/bin/bash
    set -euo pipefail

    # Start CPU stress in background
    stress --cpu 8 &
    STRESS_PID=$!

    # Setup log file
    LOGFILE="test_runs_$(date +%s).log"
    echo "Logging to $LOGFILE"

    # Run tests until one fails
    RUN=1
    while cargo test my_test > "$LOGFILE" 2>&1; do
        echo "Run $RUN passed"
        ((RUN++))
    done

    # Cleanup and report
    kill $STRESS_PID
    echo "Test failed on run $RUN"
    echo "Last 20 lines of output:"
    tail -n 20 "$LOGFILE"
    echo "Full log: $LOGFILE"
    ```

    ### Modified Script (flasky_modified.sh)
    ```bash
    #!/bin/bash
    set -euo pipefail

    # Start CPU stress in background
    stress --cpu 8 &
    STRESS_PID=$!

    # Setup log file
    LOGFILE="test_runs_$(date +%s).log"
    echo "Logging to $LOGFILE"

    # Run tests until one fails
    RUN=1
    # "$@" expands to all arguments passed to the script
    while "$@"  > "$LOGFILE" 2>&1; do
        echo "Run $RUN passed"
        ((RUN++))
    done

    # Cleanup and report
    kill $STRESS_PID
    echo "Test failed on run $RUN"
    echo "Last 20 lines of output:"
    tail -n 20 "$LOGFILE"
    echo "Full log: $LOGFILE"
    ```
    ### Demo
    ```console
    rightbear@Rightbear:~/rust_test$ ./flasky_modified.sh cargo test my_test
    stress: info: [23151] dispatching hogs: 8 cpu, 0 io, 0 vm, 0 hdd
    Logging to test_runs_1776494298.log
    Run 1 passed
    Run 2 passed
    Run 3 passed
    Run 4 passed
    Run 5 passed
    Run 6 passed
    Run 7 passed
    Run 8 passed
    Run 9 passed
    Run 10 passed
    ...
    ```

12. Modify the flaky test script from the lecture to accept the test command as an argument instead of hardcoding `cargo test my_test`. (Hint: `$1` or `$@`). See [Special Parameters](https://www.gnu.org/software/bash/manual/html_node/Special-Parameters.html).

    ## **Answer** 
    ### Demo
    ```console
    rightbear@Rightbear:~$ cp notes.txt notes_$(date +%Y-%m-%d).txt
    rightbear@Rightbear:~$ ls notes*
    notes.txt  notes_2026-04-16.txt
    ```

13. Use pipes to find the 5 most common file extensions in your home directory. (Hint: combine `find`, `grep` or `sed` or `awk`, `sort`, `uniq -c`, and `head`.)

    ## **Answer** 
    ### Command
    `find $HOME -maxdepth 1 -type f | grep -Eo '[^/]+$' | grep -Ev '^\.[^.]+$' | grep '\.' | sed -E 's/.*\.(.*)/\1/' | sort | uniq -c | sort -nrk1,1 | head -n 5 | awk '{print $2}'`

    ### Explanation
    Find the file list $\rightarrow$
    Extract the filename from each PATH (e.g., `file.ext`, `.file`, `FILE`, `.file.ext`) $\rightarrow$
    Exclude hidden files without extensions (e.g., `.file`) $\rightarrow$
    Exclude regular files without extensions (e.g., `FILE`) $\rightarrow$
    Extract the file extensions $\rightarrow$
    Sort the extensions $\rightarrow$
    Count the occurrences of each extension $\rightarrow$
    Sort by the frequency of extensions $\rightarrow$
    Get the top 5 extensions $\rightarrow$
    Print only the extensions without the counts.

    ### Demo
    ```console
    rightbear@Rightbear:~$ find $HOME -maxdepth 1 -type f \
    | grep -Eo '[^/]+$' | grep -Ev '^\.[^.]+$' \
    | grep '\.' | sed -E 's/.*\.(.*)/\1/' \
    | sort | uniq -c | sort -nrk1,1 \
    | head -n 5 | awk '{print $2}'
    txt
    sh
    log
    json
    deb
    ```

14. `xargs` converts lines from stdin into command arguments. Use `find` and `xargs` together (not `find -exec`) to find all `.sh` files in a directory and count the lines in each with `wc -l`. Bonus: make it handle filenames with spaces. (Hint: `-print0` and `-0`). See `man xargs`.

    ## **Answer** 
    ### Command
    `find . -maxdepth 1 -name "*.sh" -type f -print0 | xargs -0 wc -l`

    ### Explanation
    Find the file list with the `-print0` flag to terminate each filename with a null character (`\0`) $\rightarrow$
    Use `xargs` with the `-0` flag to pass the filenames as arguments to the `wc` command, ensuring that filenames containing special characters like spaces(`' '`), tabs(`\t`), or newlines(`\n`) are handled correctly (excluding the null character, which is treated as the argument delimiter).

    ### Demo
    ```console
    rightbear@Rightbear:~$ find . -maxdepth 1 -name "*.sh" -type f -print0 \
    | xargs -0 wc -l
    6 ./testset.sh
    15 ./check.sh
    0 ./file with space.sh
    24 ./flasky_modified.sh
    14 ./cleanup.sh
    15 ./checkfile.sh
    74 total
    ```

15. Use `curl` to fetch the HTML of the course website (`https://missing.csail.mit.edu/`) and pipe it to `grep` to count how many lectures are listed. (Hint: look for a pattern that appears once per lecture; use `curl -s` to silence the progress output.)

    ## **Answer** 
    ### Command
    `curl -s https://missing.csail.mit.edu/ | grep -E "/2026/.+/" | grep -v "nav-link" | wc -l`

    ### Explanation
    Find the file list with the `-print0` flag to terminate each filename with a null character (`\0`) $\rightarrow$
    Use `xargs` with the `-0` flag to pass the filenames as arguments to the `wc` command, ensuring that filenames containing special characters like spaces(`' '`), tabs(`\t`), or newlines(`\n`) are handled correctly (excluding the null character, which is treated as the argument delimiter).

    ### Demo
    ```console
    rightbear@Rightbear:~$ curl -s https://missing.csail.mit.edu/ \
    | grep -E "/2026/.+/" \
    | grep -v "nav-link" \
    | wc -l
    9
    ```

    ## Supplemet (Extract the list of all course names)
    ### Command
    `curl -s https://missing.csail.mit.edu/ | grep -E "/2026/.+/" | grep -v "nav-link" | sed 's/<[^>]*>//g'`

    ### Explanation
    Get the HTML content of the URL $\rightarrow$
    Extract the `<span>` elements related to the course list from the HTML $\rightarrow$
    Remove the title elements from the `<span>` group, keeping only the other elements that contain course names $\rightarrow$
    Extract the course names from the remaining elements.

    ### Demo
    ```console
    rightbear@Rightbear:~$  curl -s https://missing.csail.mit.edu/ \
    | grep -E "/2026/.+/" \
    | grep -v "nav-link" \
    | sed 's/<[^>]*>//g'
                Course Overview + Introduction to the Shell
                Command-line Environment
                Development Environment and Tools
                Debugging and Profiling
                Version Control and Git
                Packaging and Shipping Code
                Agentic Coding
                Beyond the Code
                Code Quality
    ```

16. [`jq`](https://jqlang.github.io/jq/) is a powerful tool for processing JSON data. Fetch the sample data at `https://microsoftedge.github.io/Demos/json-dummy-data/64KB.json` with `curl` and use `jq` to extract just the names of people whose version is greater than 6. (Hint: pipe to `jq .` first to see the structure; then try `jq '.[] | select(...) | .name'`)

    ## **Answer** 
    ### Command
    `curl -s https://microsoftedge.github.io/Demos/json-dummy-data/64KB.json | jq '.[] | select(.version | . > 6) | .name'`

    ### Explanation
    Get the JSON file from the URL $\rightarrow$
    Filter specific JSON elements based on conditions
    (Condition: Expand arrays $\rightarrow$ Set conditions $\rightarrow$ Extract fields)

    ### Demo
    ```console
    rightbear@Rightbear:~$ curl -s https://microsoftedge.github.io/Demos/json-dummy-data/64KB.json \
    | jq '.[] | select(.version | . > 6) | .name'
    "Adeel Solangi"
    "Aamir Solangi"
    "Adil Eli"
    "Adil Abro"
    "Antía Sixirei"
    "Aygul Mutellip"
    "Celtia Anes"
    "George Mifsud"
    "Dialè Meso"
    "Doreen Bartolo"
    "Ali Ayaz"
    "Guzelnur Polat"
    "John Falzon"
    "Marcos Amboade"
    "Shafqat Memon"
    "Meladi Papo"
    "Sabela Veloso"
    "Madule Ledimo"
    "Philip Camilleri"
    "Hunadi Makgatho"
    "Charmaine Abela"
    "Tumelò Letamo"
    "Tegra Núnez"
    "Dilnur Qeyser"
    "Iago Peirallo"
    "Josephine Balzan"
    "Mmathabò Mojapelo"
    "Kgabo Lerumo"
    "Lawrence Scicluna"
    "Joseph Grech"
    "Lesetja Theko"
    "Martiño Arxíz"
    "Malehumò Ledwaba"
    "Lajwanti Kumari"
    "Maria Sammut"
    "Matome Molamo"
    "Noa Ervello"
    "sayama Amir"
    "Mariña Quintá"
    "Memet Tursun"
    "Rashid Rajput"
    "Nicholas Micallef"
    "Peter Zammit"
    "Maname Mohlare"
    "Tshepè Mobu"
    "Monica Lohana"
    "Joanne Scerri"
    "Ratanang Maphutha"
    "Thobile Mbele"
    "Kristján Kristjánsson"
    "Stefán Stefánsson"
    "Preeti Rajdan"
    "Sanjay Trivedi"
    "Damir Benic"
    "Sigrún Kristjánsdóttir"
    "Rajesh Santoshi"
    "Mxolisi Mhlongo"
    "Monty Dubey"
    "Richa Choukse"
    "Ingibjörg Ólafsdóttir"
    "Mogorosi Bakwena"
    "Ronak Gupta"
    "Chetana Hegde"
    "Mohan Pandey"
    "Haris Osmanovic"
    "Kenosi Kwenaemang"
    "Adnan Spahic"
    "Kabelo Morwe"
    "Einar Einarsson"
    "Sigríður Einarsdóttir"
    "Sonu Jain"
    "Boitumelo Ngwako"
    "Modise Tau"
    "Gunnar Gunnarsson"
    "Kgosietsile Bogatsu"
    "Monika Nayak"
    "Lucky Shastry"
    "Raju Rathore"
    "Meenakshi Benjaree"
    "Samir Simic"
    "Prashant Chourey"
    "Prakash Malviya"
    "Ivana Kalic"
    "Denis Terzic"
    "Bukhosi Bhengu"
    "Siyabonga Sithole"
    "Rohini Vasav"
    "Sunil Kapoor"
    "Zamokuhle Zulu"
    ```

17. `awk` can filter lines based on column values and manipulate output. For example, `awk '$3 ~ /pattern/ {$4=""; print}'` prints only lines where the third column matches `pattern`, while omitting the fourth column. Write an `awk` command that prints only lines where the second column is greater than 100, and swaps the first and third columns. Test with: `printf 'a 50 x\nb 150 y\nc 200 z\n'`

    ## **Answer** 
    ### Command
    `printf 'a 50 x\nb 150 y\nc 200 z\n' | awk '$2 > 100 {TEMP = $3; $3 = $1; $1 = TEMP; print;}'`

    ### Demo
    ```console
    rightbear@Rightbear:~$ printf 'a 50 x\nb 150 y\nc 200 z\n' \
    | awk '$2 > 100 {TEMP = $3; $3 = $1; $1 = TEMP; print;}'
    y 150 b
    z 200 c
    ```

18. Dissect the SSH log pipeline from the lecture: what does each step do? Then build something similar to find your most-used shell commands from `~/.bash_history` (or `~/.zsh_history`).

    ## **Answer** 
    ### Original Command
    `ssh myserver 'journalctl -u sshd -b-1 | grep "Disconnected from"' | sed -E 's/.*Disconnected from .* user (.*) [^ ]+ port.*/\1/' | sort | uniq -c | sort -nk1,1 | tail -n10 | awk '{print $2}' | paste -sd,`

    ### Explanation

    Log in to the remote host "myserver" and retrieve the `sshd` service logs from the previous boot (flag `-b -1`) $\rightarrow$
    Filter for logs containing the keyword "Disconnected from" $\rightarrow$
    Extract the usernames from the logs $\rightarrow$
    Sort the usernames alphabetically $\rightarrow$
    Generate a statistics table to count the occurrences of each user $\rightarrow$
    Sort the users by their occurrence counts $\rightarrow$
    Filter for the top 10 users with the most occurrences $\rightarrow$
    Extract only the usernames from the statistics table $\rightarrow$
    Merge the multiple lines into a single line separated by commas (`,`).

    ### Original Demo
    ```console
    missing:~$ ssh myserver 'journalctl -u sshd -b-1 | grep "Disconnected from"' \
    | sed -E 's/.*Disconnected from .* user (.*) [^ ]+ port.*/\1/' \
    | sort | uniq -c \
    | sort -nk1,1 | tail -n10 \
    | awk '{print $2}' | paste -sd,
    postgres,mysql,oracle,dell,ubuntu,inspur,test,admin,user,root
    ```

    ### Modified Version Command
    `cat ~/.bash_history  | sort | uniq -c  | sort -nk1,1 | tail -n10 | awk '{print $2}' | paste -sd,`

    ### Modified Version Demo
    ```console
    rightbear@Rightbear:~$ cat ~/.bash_history | sort | \
    uniq -c  | sort -nk1,1 | tail -n10 \
    | awk '{print $2}' | paste -sd,
    vim,bash,cd,cat,jobs,exit,sudo,tldr,ls,ls
    ```