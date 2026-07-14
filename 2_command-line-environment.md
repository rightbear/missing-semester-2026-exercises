# Exercises

> Testing Environment: 
> 1. Windows11 + WSL2 (Ubuntu 24.04 LTS)
> 2. Google Compute Engine (Ubuntu 24.04 LTS)

## Arguments and Globs

1. You might see commands like `cmd --flag -- --notaflag`. The `--` is a special argument that tells the program to stop parsing flags. Everything after `--` is treated as a positional argument. Why might this be useful? Try running `touch -- -myfile` and then removing it without `--`.

    ## **Answer**
    ### Explanation
    We need to use the double-hyphen (`--`) anytime our non-option arguments start with a hyphen. If we don't terminate option processing, commands will try to interpret non-option arguments as options, and most likely fail. For instance, the double-hyphen can prevent command parsing errors when files start with `-` (e.g., a file named `-myfilet`). 

    ### Demo
    ```console
    rightbear@Rightbear:~$ touch -- -myfile
    rightbear@Rightbear:~$ touch -myfile
    touch: invalid option -- 'y'
    Try 'touch --help' for more information.
    ```

2. Read [`man ls`](https://www.man7.org/linux/man-pages/man1/ls.1.html) and write an `ls` command that lists files in the following manner:
    - Includes all files, including hidden files
    - Sizes are listed in human readable format (e.g. 454M instead of 454279954)
    - Files are ordered by recency
    - Output is colorized

    A sample output would look like this:

    ```
    -rw-r--r--   1 user group 1.1M Jan 14 09:53 baz
    drwxr-xr-x   5 user group  160 Jan 14 09:53 .
    -rw-r--r--   1 user group  514 Jan 14 06:42 bar
    -rw-r--r--   1 user group 106M Jan 13 12:12 foo
    drwx------+ 47 user group 1.5K Jan 12 18:08 ..
    ```

    ## **Answer**
    ### Demo
    ```console
    rightbear@Rightbear:~$ man ls
    …
        -a, --all
                do not ignore entries starting with .
    …
        --color[=WHEN]
                color the output WHEN; more info below
    …
        -h, --human-readable
                with -l and -s, print sizes like 1K 234M 2G etc.
    …
        -l     use a long listing format
    …
        -t     sort by time, newest first; see --time
    …

        The WHEN argument defaults to 'always' and can also be 'auto' or 'never'.q
        Using  color  to  distinguish  file  types  is  disabled  both  by  default  and  with --color=never.  With --color=auto, ls emits color codes only when standard output is connected to a terminal.  The LS_COLORS environment variable can change the settings.  Use the dircolors(1) command to set it.
    …

    rightbear@Rightbear:~$ ls -laht --color=auto
    total 100K
    drwxr-x--- 11 rightbear rightbear 4.0K Apr 20 23:17 .
    -rw-r--r--  1 rightbear rightbear    0 Apr 20 23:17 bar
    -rw-r--r--  1 rightbear rightbear    0 Apr 20 23:17 baz
    -rw-r--r--  1 rightbear rightbear    0 Apr 20 23:17 foo
    -rw-rw-r--  1 rightbear rightbear    0 Apr 20 23:14 .motd_shown
    -rw-------  1 rightbear rightbear 5.9K Apr 18 17:26 .bash_history
    drwxr-xr-x  3 rightbear rightbear 4.0K Apr 18 16:32 .dotnet
    -rw-r--r--  1 rightbear rightbear  183 Apr 18 16:31 .wget-hsts
    -rw-------  1 rightbear rightbear  13K Apr 18 14:40 .viminfo
    drwxr-xr-x  3 rightbear rightbear 4.0K Apr 18 14:24 .cargo
    drwxr-xr-x  6 rightbear rightbear 4.0K Apr 18 14:19 .rustup
    -rw-r--r--  1 rightbear rightbear 3.9K Apr 18 14:19 .bashrc
    -rw-r--r--  1 rightbear rightbear  828 Apr 18 14:19 .profile
    -rw-------  1 rightbear rightbear   20 Apr 16 23:27 .lesshst
    -rw-r--r--  1 rightbear rightbear   20 Dec 25 17:19 test.txt
    -rw-------  1 rightbear rightbear   10 Oct  5  2025 .node_repl_history
    drwxr-xr-x  8 rightbear rightbear 4.0K Oct  5  2025 .nvm
    drwxr-xr-x  2 rightbear rightbear 4.0K Oct  5  2025 repos
    drwx------  2 rightbear rightbear 4.0K Oct  5  2025 .ssh
    -rw-r--r--  1 rightbear rightbear  108 Oct  5  2025 .gitconfig
    -rw-r--r--  1 rightbear rightbear    0 Oct  5  2025 .sudo_as_admin_successful
    drwx------  3 rightbear rightbear 4.0K Oct  5  2025 .cache
    drwxr-xr-x  5 rightbear rightbear 4.0K Oct  5  2025 .vscode-server
    -rw-r--r--  1 rightbear rightbear  220 Oct  5  2025 .bash_logout
    drwxr-xr-x  3 root      root      4.0K Oct  5  2025 ..
    ```

3. Process substitution `<(command)` lets you use a command's output as if it were a file. Use `diff` with process substitution to compare the output of `printenv` and `export`. Why are they different? (Hint: try `diff <(printenv | sort) <(export | sort)`).

    ## **Answer**
    ### Demo
    ```console
    rightbear@Rightbear:~$ diff <(printenv | sort) <(export | sort)
    1,30c1,30
    < DBUS_SESSION_BUS_ADDRESS=unix:path=/run/user/1000/bus
    < DISPLAY=:0
    < HOME=/home/rightbear
    < HOSTTYPE=x86_64
    < LANG=C.UTF-8
    < LESSCLOSE=/usr/bin/lesspipe %s %s
    < LESSOPEN=| /usr/bin/lesspipe %s
    < LOGNAME=rightbear
    < LS_COLORS=rs=0:di=01;34:ln=01;36:mh=00:pi=40;33:so=01;35:do=01;35:bd=40;33;01:cd=40;33;01:or=40;31;01:mi=00:su=37;41:sg=30;43:ca=00:tw=30;42:ow=34;42:st=37;44:ex=01;32:*.tar=01;31:*.tgz=01;31:*.arc=01;31:*.arj=01;31:*.taz=01;31:*.lha=01;31:*.lz4=01;31:*.lzh=01;31:*.lzma=01;31:*.tlz=01;31:*.txz=01;31:*.tzo=01;31:*.t7z=01;31:*.zip=01;31:*.z=01;31:*.dz=01;31:*.gz=01;31:*.lrz=01;31:*.lz=01;31:*.lzo=01;31:*.xz=01;31:*.zst=01;31:*.tzst=01;31:*.bz2=01;31:*.bz=01;31:*.tbz=01;31:*.tbz2=01;31:*.tz=01;31:*.deb=01;31:*.rpm=01;31:*.jar=01;31:*.war=01;31:*.ear=01;31:*.sar=01;31:*.rar=01;31:*.alz=01;31:*.ace=01;31:*.zoo=01;31:*.cpio=01;31:*.7z=01;31:*.rz=01;31:*.cab=01;31:*.wim=01;31:*.swm=01;31:*.dwm=01;31:*.esd=01;31:*.avif=01;35:*.jpg=01;35:*.jpeg=01;35:*.mjpg=01;35:*.mjpeg=01;35:*.gif=01;35:*.bmp=01;35:*.pbm=01;35:*.pgm=01;35:*.ppm=01;35:*.tga=01;35:*.xbm=01;35:*.xpm=01;35:*.tif=01;35:*.tiff=01;35:*.png=01;35:*.svg=01;35:*.svgz=01;35:*.mng=01;35:*.pcx=01;35:*.mov=01;35:*.mpg=01;35:*.mpeg=01;35:*.m2v=01;35:*.mkv=01;35:*.webm=01;35:*.webp=01;35:*.ogm=01;35:*.mp4=01;35:*.m4v=01;35:*.mp4v=01;35:*.vob=01;35:*.qt=01;35:*.nuv=01;35:*.wmv=01;35:*.asf=01;35:*.rm=01;35:*.rmvb=01;35:*.flc=01;35:*.avi=01;35:*.fli=01;35:*.flv=01;35:*.gl=01;35:*.dl=01;35:*.xcf=01;35:*.xwd=01;35:*.yuv=01;35:*.cgm=01;35:*.emf=01;35:*.ogv=01;35:*.ogx=01;35:*.aac=00;36:*.au=00;36:*.flac=00;36:*.m4a=00;36:*.mid=00;36:*.midi=00;36:*.mka=00;36:*.mp3=00;36:*.mpc=00;36:*.ogg=00;36:*.ra=00;36:*.wav=00;36:*.oga=00;36:*.opus=00;36:*.spx=00;36:*.xspf=00;36:*~=00;90:*#=00;90:*.bak=00;90:*.crdownload=00;90:*.dpkg-dist=00;90:*.dpkg-new=00;90:*.dpkg-old=00;90:*.dpkg-tmp=00;90:*.old=00;90:*.orig=00;90:*.part=00;90:*.rej=00;90:*.rpmnew=00;90:*.rpmorig=00;90:*.rpmsave=00;90:*.swp=00;90:*.tmp=00;90:*.ucf-dist=00;90:*.ucf-new=00;90:*.ucf-old=00;90:
    < MOTD_SHOWN=update-motd
    < NAME=Rightbear
    < NVM_BIN=/home/rightbear/.nvm/versions/node/v22.20.0/bin
    < NVM_CD_FLAGS=
    < NVM_DIR=/home/rightbear/.nvm
    < NVM_INC=/home/rightbear/.nvm/versions/node/v22.20.0/include/node
    < PATH=/home/rightbear/.cargo/bin:/home/rightbear/.nvm/versions/node/v22.20.0/bin:/usr/local/sbin:/usr/local/bin:/usr/sbin:/usr/bin:/sbin:/bin:/usr/games:/usr/local/games:/usr/lib/wsl/lib:/mnt/c/Program Files/Alacritty/:/mnt/c/Program Files (x86)/VMware/VMware Workstation/bin/:/mnt/c/Windows/system32:/mnt/c/Windows:/mnt/c/Windows/System32/Wbem:/mnt/c/Windows/System32/WindowsPowerShell/v1.0/:/mnt/c/Windows/System32/OpenSSH/:/mnt/c/Program Files (x86)/NVIDIA Corporation/PhysX/Common:/mnt/c/Program Files/NVIDIA Corporation/NVIDIA App/NvDLISR:/mnt/c/Program Files/dotnet/:/mnt/c/Users/jjj17/AppData/Local/Microsoft/WindowsApps:/mnt/c/Users/jjj17/AppData/Local/GitHubDesktop/bin:/mnt/c/ProgramData/jjj17/GitHubDesktop/bin:/mnt/c/Users/jjj17/AppData/Local/Programs/Microsoft VS Code/bin:/snap/bin
    < PULSE_SERVER=unix:/mnt/wslg/PulseServer
    < PWD=/home/rightbear
    < SHELL=/bin/bash
    < SHLVL=1
    < TERM=xterm-256color
    < USER=rightbear
    < WAYLAND_DISPLAY=wayland-0
    < WSL2_GUI_APPS_ENABLED=1
    < WSLENV=
    < WSL_DISTRO_NAME=Ubuntu
    < WSL_INTEROP=/run/WSL/36189_interop
    < XDG_DATA_DIRS=/usr/local/share:/usr/share:/var/lib/snapd/desktop
    < XDG_RUNTIME_DIR=/run/user/1000/
    < _=/usr/bin/printenv
    ---
    > declare -x DBUS_SESSION_BUS_ADDRESS="unix:path=/run/user/1000/bus"
    > declare -x DISPLAY=":0"
    > declare -x HOME="/home/rightbear"
    > declare -x HOSTTYPE="x86_64"
    > declare -x LANG="C.UTF-8"
    > declare -x LESSCLOSE="/usr/bin/lesspipe %s %s"
    > declare -x LESSOPEN="| /usr/bin/lesspipe %s"
    > declare -x LOGNAME="rightbear"
    > declare -x LS_COLORS="rs=0:di=01;34:ln=01;36:mh=00:pi=40;33:so=01;35:do=01;35:bd=40;33;01:cd=40;33;01:or=40;31;01:mi=00:su=37;41:sg=30;43:ca=00:tw=30;42:ow=34;42:st=37;44:ex=01;32:*.tar=01;31:*.tgz=01;31:*.arc=01;31:*.arj=01;31:*.taz=01;31:*.lha=01;31:*.lz4=01;31:*.lzh=01;31:*.lzma=01;31:*.tlz=01;31:*.txz=01;31:*.tzo=01;31:*.t7z=01;31:*.zip=01;31:*.z=01;31:*.dz=01;31:*.gz=01;31:*.lrz=01;31:*.lz=01;31:*.lzo=01;31:*.xz=01;31:*.zst=01;31:*.tzst=01;31:*.bz2=01;31:*.bz=01;31:*.tbz=01;31:*.tbz2=01;31:*.tz=01;31:*.deb=01;31:*.rpm=01;31:*.jar=01;31:*.war=01;31:*.ear=01;31:*.sar=01;31:*.rar=01;31:*.alz=01;31:*.ace=01;31:*.zoo=01;31:*.cpio=01;31:*.7z=01;31:*.rz=01;31:*.cab=01;31:*.wim=01;31:*.swm=01;31:*.dwm=01;31:*.esd=01;31:*.avif=01;35:*.jpg=01;35:*.jpeg=01;35:*.mjpg=01;35:*.mjpeg=01;35:*.gif=01;35:*.bmp=01;35:*.pbm=01;35:*.pgm=01;35:*.ppm=01;35:*.tga=01;35:*.xbm=01;35:*.xpm=01;35:*.tif=01;35:*.tiff=01;35:*.png=01;35:*.svg=01;35:*.svgz=01;35:*.mng=01;35:*.pcx=01;35:*.mov=01;35:*.mpg=01;35:*.mpeg=01;35:*.m2v=01;35:*.mkv=01;35:*.webm=01;35:*.webp=01;35:*.ogm=01;35:*.mp4=01;35:*.m4v=01;35:*.mp4v=01;35:*.vob=01;35:*.qt=01;35:*.nuv=01;35:*.wmv=01;35:*.asf=01;35:*.rm=01;35:*.rmvb=01;35:*.flc=01;35:*.avi=01;35:*.fli=01;35:*.flv=01;35:*.gl=01;35:*.dl=01;35:*.xcf=01;35:*.xwd=01;35:*.yuv=01;35:*.cgm=01;35:*.emf=01;35:*.ogv=01;35:*.ogx=01;35:*.aac=00;36:*.au=00;36:*.flac=00;36:*.m4a=00;36:*.mid=00;36:*.midi=00;36:*.mka=00;36:*.mp3=00;36:*.mpc=00;36:*.ogg=00;36:*.ra=00;36:*.wav=00;36:*.oga=00;36:*.opus=00;36:*.spx=00;36:*.xspf=00;36:*~=00;90:*#=00;90:*.bak=00;90:*.crdownload=00;90:*.dpkg-dist=00;90:*.dpkg-new=00;90:*.dpkg-old=00;90:*.dpkg-tmp=00;90:*.old=00;90:*.orig=00;90:*.part=00;90:*.rej=00;90:*.rpmnew=00;90:*.rpmorig=00;90:*.rpmsave=00;90:*.swp=00;90:*.tmp=00;90:*.ucf-dist=00;90:*.ucf-new=00;90:*.ucf-old=00;90:"
    > declare -x MOTD_SHOWN="update-motd"
    > declare -x NAME="Rightbear"
    > declare -x NVM_BIN="/home/rightbear/.nvm/versions/node/v22.20.0/bin"
    > declare -x NVM_CD_FLAGS=""
    > declare -x NVM_DIR="/home/rightbear/.nvm"
    > declare -x NVM_INC="/home/rightbear/.nvm/versions/node/v22.20.0/include/node"
    > declare -x OLDPWD
    > declare -x PATH="/home/rightbear/.cargo/bin:/home/rightbear/.nvm/versions/node/v22.20.0/bin:/usr/local/sbin:/usr/local/bin:/usr/sbin:/usr/bin:/sbin:/bin:/usr/games:/usr/local/games:/usr/lib/wsl/lib:/mnt/c/Program Files/Alacritty/:/mnt/c/Program Files (x86)/VMware/VMware Workstation/bin/:/mnt/c/Windows/system32:/mnt/c/Windows:/mnt/c/Windows/System32/Wbem:/mnt/c/Windows/System32/WindowsPowerShell/v1.0/:/mnt/c/Windows/System32/OpenSSH/:/mnt/c/Program Files (x86)/NVIDIA Corporation/PhysX/Common:/mnt/c/Program Files/NVIDIA Corporation/NVIDIA App/NvDLISR:/mnt/c/Program Files/dotnet/:/mnt/c/Users/jjj17/AppData/Local/Microsoft/WindowsApps:/mnt/c/Users/jjj17/AppData/Local/GitHubDesktop/bin:/mnt/c/ProgramData/jjj17/GitHubDesktop/bin:/mnt/c/Users/jjj17/AppData/Local/Programs/Microsoft VS Code/bin:/snap/bin"
    > declare -x PULSE_SERVER="unix:/mnt/wslg/PulseServer"
    > declare -x PWD="/home/rightbear"
    > declare -x SHELL="/bin/bash"
    > declare -x SHLVL="1"
    > declare -x TERM="xterm-256color"
    > declare -x USER="rightbear"
    > declare -x WAYLAND_DISPLAY="wayland-0"
    > declare -x WSL2_GUI_APPS_ENABLED="1"
    > declare -x WSLENV=""
    > declare -x WSL_DISTRO_NAME="Ubuntu"
    > declare -x WSL_INTEROP="/run/WSL/36189_interop"
    > declare -x XDG_DATA_DIRS="/usr/local/share:/usr/share:/var/lib/snapd/desktop"
    > declare -x XDG_RUNTIME_DIR="/run/user/1000/"
    ```

    ### Explanation
    `printenv` command outputs environment variables in a "raw" format: `KEY=VALUE`. (e.g. `HOME=/home/rightbear`) \
    `export` command, as a Shell built-in, outputs environment variables in a format that the Shell can re-execute as a script. It adds the prefix `declare -x` and wraps values in double quotes. (e.g. `declare -x HOME="/home/rightbear"`) \
    Because `diff` command is a literal line-by-line comparison tool, it sees `HOME=...` and `declare -x HOME="..."` as completely different strings.

## Environment Variables

1. Write bash functions `marco` and `polo` that do the following: whenever you execute `marco` the current working directory should be saved in some manner, then when you execute `polo`, no matter what directory you are in, `polo` should `cd` you back to the directory where you executed `marco`. For ease of debugging you can write the code in a file `marco.sh` and (re)load the definitions to your shell by executing `source marco.sh`.

    ## **Answer**
    ### Script (marco.sh)
    ```bash
    #!/bin/bash

    # Save the current working directory
    marco()
    {
            # Use 'export' to ensure the variable is not just local to this shell,
            # but also accessible to any child processes or sub-shells started from here.
            export CURRENT_DIR=$(pwd)
            echo "Directory $CURRENT_DIR saved!"
    }

    # polo: Change the directory to the one saved by marco
    polo()
    {
            # Check if marco has been executed
            if [ -z "$CURRENT_DIR" ]; then
                    echo "Error: You haven't executed 'marco' yet!"
            else
                    cd "$CURRENT_DIR"
            fi
    }
    ```

    ### Demo
    ```console
    rightbear@Rightbear:~$ source marco.sh
    rightbear@Rightbear:~$ pwd
    /home/rightbear
    rightbear@Rightbear:~$ polo
    Please execute marco before executing polo.
    rightbear@Rightbear:~$ marco
    rightbear@Rightbear:~$ pwd
    /home/rightbear
    rightbear@Rightbear:~$ echo $CURRENT_DIR
    /home/rightbear
    rightbear@Rightbear:~$ cd test_marco
    rightbear@Rightbear:~/test_marco$ pwd
    /home/rightbear/test_marco
    rightbear@Rightbear:~/test_marco$ marco
    rightbear@Rightbear:~/test_marco$ echo $CURRENT_DIR
    /home/rightbear/test_marco
    rightbear@Rightbear:~/test_marco$ cd ~
    rightbear@Rightbear:~$ pwd
    /home/rightbear
    rightbear@Rightbear:~$ polo
    rightbear@Rightbear:~/test_marco$ pwd
    /home/rightbear/test_marco
    rightbear@Rightbear:~/test_marco$ cd ~
    rightbear@Rightbear:~$ rm -r test_marco/
    rightbear@Rightbear:~$ polo
    -bash: cd: /home/rightbear/test_marco: No such file or directory
    rightbear@Rightbear:~$
    ```

## Return Codes

1. Say you have a command that fails rarely. In order to debug it you need to capture its output but it can be time consuming to get a failure run. Write a bash script that runs the following script until it fails and captures its standard output and error streams to files and prints everything at the end. Bonus points if you can also report how many runs it took for the script to fail.

    ```bash
    #!/usr/bin/env bash

    n=$(( RANDOM % 100 ))

    if [[ $n -eq 42 ]]; then
       echo "Something went wrong"
       >&2 echo "The error was using magic numbers"
       exit 1
    fi

    echo "Everything went according to plan"
    ```

    ## **Answer**
    ### Script (return_code_practice.sh)
    ```bash
    #!/usr/bin/env bash

    # Naming the script in the description of problem as test_fail.sh
    TEST_SH="test_fail.sh"

    # Naming the file to record the testing result as record_fail.log
    RECORD_LOG="record_fail.log"

    # Initialize the counter variable, recording file and $? value 
    round=0
    : > $RECORD_LOG
    true

    # Until Loop continues as long as the return code of testing command is 0
    until [[ "$?" -ne 0 ]];
    do
        round=$((round+1))

        # Print the progress of until loops
        echo -ne "The script has been run $round rounds\r"

        # This line of code determine $? value in the next round of loops
        ./$TEST_SH >> $RECORD_LOG 2>&1
    done

    # Output the result of the whole testing procedure
    cat $RECORD_LOG
    echo "The script takes $round runs to fail"
    ```

    ### Demo
    ```console
    rightbear@Rightbear:~$ ./return_code_practice.sh
    Everything went according to plan
    Everything went according to plan
    Everything went according to plan
    Everything went according to plan
    Everything went according to plan
    Everything went according to plan
    Everything went according to plan
    Everything went according to plan
    Everything went according to plan
    Everything went according to plan
    Something went wrong
    The error was using magic numbers
    The script takes 11 runs to fail
    ```

## Signals and Job Control

1. Start a `sleep 10000` job in a terminal, background it with `Ctrl-Z` and continue its execution with `bg`. Now use [`pgrep`](https://www.man7.org/linux/man-pages/man1/pgrep.1.html) to find its pid and [`pkill`](https://man7.org/linux/man-pages/man1/pgrep.1.html) to kill it without ever typing the pid itself. (Hint: use the `-lf` flags).

    ## **Answer**
    ### Explanation
    The combination of `-f` and `-l` flags can make finding and terminating specific processes both precise and safe, without ever needing to manually type the PID.

    ### Demo
    ```console
    rightbear@Rightbear:~$ man pkill
    …
       -f, --full
              The pattern is normally only matched against the process name.  When -f is set, the full command line is used.
    …
       -l, --list-name
              List the process name as well as the process ID.  (pgrep only.)
    …

    rightbear@Rightbear:~$ sleep 10000
    ^Z
    [1]+  Stopped                 sleep 10000
    rightbear@Rightbear:~$ bg
    [1]+ sleep 10000 &
    rightbear@Rightbear:~$ pgrep -lf "sleep 10000"
    1320 sleep
    rightbear@Rightbear:~$ pkill -f "sleep 10000"
    [1]+  Terminated              sleep 10000
    ```

2. Say you don't want to start a process until another completes. How would you go about it? In this exercise, our limiting process will always be `sleep 60 &`. One way to achieve this is to use the [`wait`](https://www.man7.org/linux/man-pages/man1/wait.1p.html) command. Try launching the sleep command and having an `ls` wait until the background process finishes.

    However, this strategy will fail if we start in a different bash session, since `wait` only works for child processes. One feature we did not discuss in the notes is that the `kill` command's exit status will be zero on success and nonzero otherwise. `kill -0` does not send a signal but will give a nonzero exit status if the process does not exist. Write a bash function called `pidwait` that takes a pid and waits until the given process completes. You should use `sleep` to avoid wasting CPU unnecessarily.

    ## **Answer**
    ### Function file (test_pidwait)
    ```bash
    pidwait() {

        # Check if PID is input as parameter
        if [ -z "$1" ]; then
            echo "Usage: pidwait <PID>"
            return 1
        fi

        local testpid=$1

        echo "Starting to monitor PID: $testpid"

        # As long as the return code of "kill -0" is not 0, keeping the while loop continuing
        while kill -0 $testpid 2>/dev/null; do
            echo "Pleasd keep waiting"
            sleep 5
        done

        echo "Process $testpid has completed!"
    }
    ```

    ### Demo1 (Terminal 1)
    ```console
    rightbear@Rightbear:~$ source test_pidwait
    rightbear@Rightbear:~$ pidwait
    Usage: pidwait <PID>
    ```

    ### Demo2 (Terminal 2)
    ```console
    rightbear@Rightbear:~$ sleep 60 &
    [1] 4390
    ```

    ### Demo3 (Terminal 1)
    ```console
    rightbear@Rightbear:~$ pidwait 4390
    Starting to monitor PID: 4390
    Pleasd keep waiting
    Pleasd keep waiting
    Pleasd keep waiting
    Pleasd keep waiting
    Pleasd keep waiting
    Pleasd keep waiting
    Pleasd keep waiting
    Pleasd keep waiting
    Pleasd keep waiting
    Pleasd keep waiting
    Pleasd keep waiting
    Pleasd keep waiting
    Process 4390 has completed!
    ```

## Files and Permissions

1. (Advanced) Write a command or script to recursively find the most recently modified file in a directory. More generally, can you list all files by recency?

    ## **Answer**
    ### Command
    `find . -type f -exec stat --format='%Y %n' {} + | sort -n | grep -Eo '[^/]+$'`

    ### Explanation
    Find all files in the current directory and compile them into a file list. The file list includes last modified time (presented in Unix seconds) and the file path of files. $\rightarrow$
    Sort the file list according to the last modified time of files. $\rightarrow$
    Extract only the portion of the file list containing the file path.

    ### Demo
    ```console
    rightbear@Rightbear:~/test_recency$ tree
    .
    ├── directory_1
    │   ├── file_1
    │   └── file_4
    ├── directory_2
    │   ├── file with space_5
    │   └── file_3
    └── file_2

    3 directories, 5 files
    rightbear@Rightbear:~/test_recency$ ls -latR
    .:
    total 16
    drwxr-xr-x  2 rightbear rightbear 4096 Apr 30 16:03 directory_2
    drwxr-xr-x  2 rightbear rightbear 4096 Apr 30 15:50 directory_1
    drwxr-xr-x  4 rightbear rightbear 4096 Apr 30 15:50 .
    drwxr-x--- 20 rightbear rightbear 4096 Apr 30 15:50 ..
    -rw-r--r--  1 rightbear rightbear    0 Apr 30 15:48 file_2

    ./directory_2:
    total 8
    drwxr-xr-x 2 rightbear rightbear 4096 Apr 30 16:03  .
    -rw-r--r-- 1 rightbear rightbear    0 Apr 30 16:03 'file with space_5'
    drwxr-xr-x 4 rightbear rightbear 4096 Apr 30 15:50  ..
    -rw-r--r-- 1 rightbear rightbear    0 Apr 30 15:49  file_3

    ./directory_1:
    total 8
    drwxr-xr-x 2 rightbear rightbear 4096 Apr 30 15:50 .
    -rw-r--r-- 1 rightbear rightbear    0 Apr 30 15:50 file_4
    drwxr-xr-x 4 rightbear rightbear 4096 Apr 30 15:50 ..
    -rw-r--r-- 1 rightbear rightbear    0 Apr 30 15:48 file_1

    rightbear@Rightbear:~/test_recency$ echo "Files modified from the newest to the oldest:" \
    ; find . -type f -exec stat --format='%Y %n' {} + \
    | sort -nr | sed 's/[^ ]* //'
    Files modified from the newest to the oldest:
    ./directory_2/file with space_5
    ./directory_1/file_4
    ./directory_2/file_3
    ./file_2
    ./directory_1/file_1

    rightbear@Rightbear:~/test_recency$ touch ./file_2
    rightbear@Rightbear:~/test_recency$ echo "Files modified from the newest to the oldest:" \
    ; find . -type f -exec stat --format='%Y %n' {} + \
    | sort -nr | sed 's/[^ ]* //'
    ./file_2
    ./directory_2/file with space_5
    ./directory_1/file_4
    ./directory_2/file_3
    ./directory_1/file_1
    ```

## Terminal Multiplexers

1. Follow this `tmux` [tutorial](https://www.hamvocke.com/blog/a-quick-and-easy-guide-to-tmux/) and then learn how to do some basic customizations following [these steps](https://www.hamvocke.com/blog/a-guide-to-customizing-your-tmux-conf/).

    ## **Answer**
    ### Configuration file (.tmux.conf)
    ```bash
    # reload config file (change file location to your the tmux.conf you want to use)

    bind r source-file ~/.tmux.conf

    # Enable mouse control (clickable windows, panes, resizable panes)

    set -g mouse on

    # don't rename windows automatically

    set-option -g allow-rename off

    # DESIGN TWEAKS

    # don't do anything when a 'bell' rings

    set -g visual-activity off
    set -g visual-bell off
    set -g visual-silence off
    setw -g monitor-activity off
    set -g bell-action none

    # clock mode

    setw -g clock-mode-colour yellow

    # copy mode

    setw -g mode-style 'fg=black bg=red bold'

    # panes

    set -g pane-border-style 'fg=red'
    set -g pane-active-border-style 'fg=yellow'

    # statusbar

    set -g status-position bottom
    set -g status-justify left
    set -g status-style 'fg=red'

    set -g status-left ''
    set -g status-left-length 10

    set -g status-right-style 'fg=black bg=yellow'
    set -g status-right '%Y-%m-%d %H:%M '
    set -g status-right-length 50

    setw -g window-status-current-style 'fg=black bg=red'
    setw -g window-status-current-format ' #I #W #F '

    setw -g window-status-style 'fg=red bg=black'
    setw -g window-status-format ' #I #[fg=white]#W #[fg=yellow]#F '

    setw -g window-status-bell-style 'fg=yellow bg=red bold'

    # messages

    set -g message-style 'fg=yellow bg=red bold'
    ```

## Aliases and Dotfiles

1. Create an alias `dc` that resolves to `cd` for when you type it wrong.

    ## **Answer**
    ### Demo
    ```console
    rightbear@Rightbear:~$ alias dc="cd"
    rightbear@Rightbear:~$ dc test
    rightbear@Rightbear:~/test$
    ```

2. Run `history | awk '{$1="";print substr($0,2)}' | sort | uniq -c | sort -n | tail -n 10` to get your top 10 most used commands and consider writing shorter aliases for them. Note: this works for Bash; if you're using ZSH, use `history 1` instead of just `history`.

    ## **Answer**
    ### Demo
    ```console
    rightbear@Rightbear:~$ history | awk '{$1="";print substr($0,2)}' | sort | uniq -c | sort -n | tail -n 10
        15 clear
        16 ls -ltr /tmp
        17 cat .bashrc
        17 top
        17 sleep 60 &
        23 ps -aux
        24 ls -ltr ~
        34 tmux ls
        34 exit
        38 sudo apt update -y
    rightbear@Rightbear:~$ cat .bashrc
    …
    # Alias definitions.
    # You may want to put all your additions into a separate file like
    # ~/.bash_aliases, instead of adding them here directly.
    # See /usr/share/doc/bash-doc/examples in the bash-doc package.

    if [ -f ~/.bash_aliases ]; then
        . ~/.bash_aliases
    fi
    …
    rightbear@Rightbear:~$ cat ~/.bash_aliases
    alias cl="clear"
    alias lstp="ls -ltr /tmp"
    alias showbrc="cat .bashrc"
    alias tp="top"
    alias slp60b="sleep 60 &"
    alias psx="ps -aux"
    alias lshome="ls -ltr ~"
    alias tmls="tmux ls"
    alias et="exit"
    alias updateall="sudo apt update -y"
    rightbear@Rightbear:~$ source ~/.bash_aliases
    ```

3. Create a folder for your dotfiles and set up version control.

    ## **Answer** 
    ### Script file (makesymlinks.sh)
    ```bash
    #!/bin/bash

    ############################

    # .make.sh
    # This script creates symlinks from the home directory to any desired dotfiles in ~/dotfiles

    ############################

    ########## Variables

    dir=~/dotfiles                    # dotfiles directory
    olddir=~/dotfiles_old             # old dotfiles backup directory
    files="bash_aliases"    # list of files/folders to symlink in homedir

    ##########

    # create dotfiles_old in homedir

    echo -n "Creating $olddir for backup of any existing dotfiles in ~ ..."
    mkdir -p $olddir
    echo "done"

    # change to the dotfiles directory

    echo -n "Changing to the $dir directory ..."
    cd $dir
    echo "done"

    # move any existing dotfiles in homedir to dotfiles_old directory, then create symlinks from the homedir to any files in the ~/dotfiles directory specified in $files

    for file in $files; do
        echo "Moving any existing dotfiles from ~ to $olddir"
        mv ~/.$file ~/dotfiles_old/
        echo "Creating symlink to $file in home directory."
        ln -s $dir/$file ~/.$file
    done
    ```

    ### Demo
    ```console
    rightbear@Rightbear:~$ mkdir ~/dotfiles
    rightbear@Rightbear:~$ cp ~/.bash_aliases ~/dotfiles/bash_aliases
    rightbear@Rightbear:~$ ls ~/dotfiles
    bash_aliases
    rightbear@Rightbear:~$ cd ~/dotfiles
    rightbear@Rightbear:~/dotfiles$ vim makesymlinks.sh
    rightbear@Rightbear:~/dotfiles$ git init
    hint: Using 'master' as the name for the initial branch. This default branch name
    hint: is subject to change. To configure the initial branch name to use in all
    hint: of your new repositories, which will suppress this warning, call:
    hint:
    hint:   git config --global init.defaultBranch <name>
    hint:
    hint: Names commonly chosen instead of 'master' are 'main', 'trunk' and
    hint: 'development'. The just-created branch can be renamed via this command:
    hint:
    hint:   git branch -m <name>
    Initialized empty Git repository in /home/rightbear/dotfiles/.git/
    rightbear@Rightbear:~/dotfiles$ git add makesymlinks.sh
    rightbear@Rightbear:~/dotfiles$ git add bash_aliases
    rightbear@Rightbear:~/dotfiles$ git commit -m 'My first Git commit of my dotfiles'
    [master (root-commit) 30008b1] My first Git commit of my dotfiles
    2 files changed, 47 insertions(+)
    create mode 100644 bash_aliases
    create mode 100644 makesymlinks.sh
    rightbear@Rightbear:~/dotfiles$ git log
    commit 30008b1ea832b48b129edb0dd4c68a55bf672204 (HEAD -> master)
    Author: connlabtest <connlabtest@gmail.com>
    Date:   Mon Jun 29 15:40:07 2026 +0800

        My first Git commit of my dotfiles

    ```

4. Add a configuration for at least one program, e.g. your shell, with some customization (to start off, it can be something as simple as customizing your shell prompt by setting `$PS1`).

    ## **Answer** 
    ### Demo (Customize bash prompt with Git info)
    ```console
    rightbear@Rightbear:~$ vim ~/.bashrc
    …
    # Source git prompt if available
    if [ -f /usr/lib/git-core/git-sh-prompt ]; then
        source /usr/lib/git-core/git-sh-prompt
    elif [ -f /usr/share/git-core/contrib/completion/git-prompt.sh ]; then
        source /usr/share/git-core/contrib/completion/git-prompt.sh
    fi

    if command -v __git_ps1 &>/dev/null; then
        GIT_PS1_SHOWDIRTYSTATE=1
        GIT_PS1_SHOWUNTRACKEDFILES=1
    fi

    __prompt_command() {
        local exit=$?

        # Colors
        local reset='\[\033[00m\]'
        local bold_green='\[\033[01;32m\]'
        local bold_blue='\[\033[01;34m\]'
        local bold_yellow='\[\033[01;33m\]'
        local bold_red='\[\033[01;31m\]'

        # Exit status indicator
        local status_color
        if [ $exit -eq 0 ]; then
            status_color="$bold_green"
        else
            status_color="$bold_red"
        fi

        # Git branch
        local git_part=""
        if command -v __git_ps1 &>/dev/null; then
            git_part="${bold_yellow}$(__git_ps1 " (%s)")${reset}"
        fi

        PS1="${bold_green}\u@\h${reset}:${bold_blue}\w${reset}${git_part} ${status_color}\$${reset} "
    }

    export PROMPT_COMMAND=__prompt_command 
    …
    rightbear@Rightbear:~$ source ~/.bashrc
    rightbear@Rightbear:~ $ cd dotfiles/
    rightbear@Rightbear:~/dotfiles (master) $ git branch --show-current
    master
    rightbear@Rightbear:~/dotfiles (master) $ git switch -c test-branch
    Switched to a new branch 'test-branch'
    rightbear@Rightbear:~/dotfiles (test-branch) $ git branch --show-current
    test-branch
    rightbear@Rightbear:~/dotfiles (test-branch) $ git switch master
    Switched to branch 'master'
    rightbear@Rightbear:~/dotfiles (master) $ git branch --show-current
    master
    ```

    ### Demo: Supplement
    ```console
    rightbear@Rightbear:~ $ vim .bash_profile
    …
    # Automatically source ~/.bashrc when the user login to the shell
    if [ -f ~/.bashrc ]; then
        source ~/.bashrc
    fi
    …
    ```

5. Set up a method to install your dotfiles quickly (and without manual effort) on a new machine. This can be as simple as a shell script that calls `ln -s` for each file, or you could use a [specialized utility](https://dotfiles.github.io/utilities/).

    ## **Answer** 
    ### Demo: Preparation (Use GNU Stow)
    ```console
    rightbear@Rightbear:~ $ sudo apt install stow
    Reading package lists... Done
    Building dependency tree... Done
    Reading state information... Done
    Suggested packages:
    doc-base
    The following NEW packages will be installed:
    stow
    0 upgraded, 1 newly installed, 0 to remove and 35 not upgraded.
    Need to get 380 kB of archives.
    After this operation, 865 kB of additional disk space will be used.
    Get:1 https://arm.seli.gic.ericsson.se/artifactory/ubuntu-2rc/ubuntu noble/universe amd64 stow all 2.3.1-1 [380 kB]
    Fetched 380 kB in 13s (29.8 kB/s)
    Selecting previously unselected package stow.
    (Reading database ... 95026 files and directories currently installed.)
    Preparing to unpack .../archives/stow_2.3.1-1_all.deb ...
    Unpacking stow (2.3.1-1) ...
    Setting up stow (2.3.1-1) ...
    Processing triggers for man-db (2.12.0-4build2) ...
    Processing triggers for install-info (7.1-3build2) …
    rightbear@Rightbear:~ $ cat ~/.bash_aliases
    alias cl="clear"
    alias lstp="ls -ltr /tmp"
    alias showbrc="cat .bashrc"
    alias tp="top"
    alias slp60b="sleep 60 &"
    alias psx="ps -aux"
    alias lshome="ls -ltr ~"
    alias tmls="tmux ls"
    alias et="exit"
    alias updateall="sudo apt update -y"
    rightbear@Rightbear:~ $ mkdir dotfiles_stow
    rightbear@Rightbear:~ $ cp ~/.bash_aliases ~/dotfiles_stow/.bash_aliases
    rightbear@Rightbear:~ $ ls -a ~/dotfiles_stow
    .  ..  .bash_aliases  .git
    rightbear@Rightbear:~ $ alias -p
    alias alert='notify-send --urgency=low -i "$([ $? = 0 ] && echo terminal || echo error)" "$(history|tail -n1|sed -e '\''s/^\s*[0-9]\+\s*//;s/[;&|]\s*alert$//'\'')"'
    alias cl='clear'
    alias egrep='egrep --color=auto'
    alias et='exit'
    alias fgrep='fgrep --color=auto'
    alias grep='grep --color=auto'
    alias l='ls -CF'
    alias la='ls -A'
    alias ll='ls -alF'
    alias ls='ls --color=auto'
    alias lshome='ls -ltr ~'
    alias lstp='ls -ltr /tmp'
    …
    rightbear@Rightbear:~ $ cd dotfiles_stow/
    rightbear@Rightbear:~/dotfiles_stow $ git init .
    hint: Using 'master' as the name for the initial branch. This default branch name
    hint: is subject to change. To configure the initial branch name to use in all
    hint: of your new repositories, which will suppress this warning, call:
    hint:
    hint:   git config --global init.defaultBranch <name>
    hint:
    hint: Names commonly chosen instead of 'master' are 'main', 'trunk' and
    hint: 'development'. The just-created branch can be renamed via this command:
    hint:
    hint:   git branch -m <name>
    Initialized empty Git repository in /home/rightbear/dotfiles_stow/.git/
    rightbear@Rightbear:~/dotfiles_stow (master #%) $ git add .
    rightbear@Rightbear:~/dotfiles_stow (master +) $ git commit -m "My first Git commit of my dotfiles with GNU Stow"
    [master (root-commit) 8a9d0c3] My first Git commit of my dotfiles with GNU Stow
    1 file changed, 10 insertions(+)
    create mode 100644 .bash_aliases

    ```

    ### Demo: Method 1 (Install dotfiles without `--adopt`)
    ```console
    rightbear@Rightbear:~/dotfiles_stow (master) $ mv ../.bash_aliases ../.bash_aliases.bkp
    rightbear@Rightbear:~/dotfiles_stow (master) $ exit
    logout
    PS C:\WINDOWS\System32> wsl ~
    rightbear@Rightbear:~ $ alias -p
    alias alert='notify-send --urgency=low -i "$([ $? = 0 ] && echo terminal || echo error)" "$(history|tail -n1|sed -e '\''s/^\s*[0-9]\+\s*//;s/[;&|]\s*alert$//'\'')"'
    alias egrep='egrep --color=auto'
    alias fgrep='fgrep --color=auto'
    alias grep='grep --color=auto'
    alias l='ls -CF'
    alias la='ls -A'
    alias ll='ls -alF'
    alias ls='ls --color=auto'
    rightbear@Rightbear:~ $ cd dotfiles_stow/
    rightbear@Rightbear:~/dotfiles_stow (master) $ stow .
    rightbear@Rightbear:~/dotfiles_stow (master) $ cd $HOME
    rightbear@Rightbear:~ $ ls -lah .bash_aliases
    lrwxrwxrwx 1 rightbear rightbear 27 Jun 30 13:42 .bash_aliases -> dotfiles_stow/.bash_aliases
    rightbear@Rightbear:~ $ source .bashrc
    rightbear@Rightbear:~ $ alias -p
    alias alert='notify-send --urgency=low -i "$([ $? = 0 ] && echo terminal || echo error)" "$(history|tail -n1|sed -e '\''s/^\s*[0-9]\+\s*//;s/[;&|]\s*alert$//'\'')"'
    alias cl='clear'
    alias egrep='egrep --color=auto'
    alias et='exit'
    alias fgrep='fgrep --color=auto'
    alias grep='grep --color=auto'
    alias l='ls -CF'
    alias la='ls -A'
    alias ll='ls -alF'
    alias ls='ls --color=auto'
    alias lshome='ls -ltr ~'
    alias lstp='ls -ltr /tmp'
    …
    ```

    ### Demo: Method 2 (Install dotfiles with `--adopt`)
    ```console
    rightbear@Rightbear:~/dotfiles_stow (master) $ rm ../.bash_aliases
    rightbear@Rightbear:~/dotfiles_stow (master) $ exit
    logout
    PS C:\WINDOWS\System32> wsl ~
    rightbear@Rightbear:~ $ alias -p
    alias alert='notify-send --urgency=low -i "$([ $? = 0 ] && echo terminal || echo error)" "$(history|tail -n1|sed -e '\''s/^\s*[0-9]\+\s*//;s/[;&|]\s*alert$//'\'')"'
    alias egrep='egrep --color=auto'
    alias fgrep='fgrep --color=auto'
    alias grep='grep --color=auto'
    alias l='ls -CF'
    alias la='ls -A'
    alias ll='ls -alF'
    alias ls='ls --color=auto'
    rightbear@Rightbear:~ $ cd dotfiles_stow/
    rightbear@Rightbear:~/dotfiles_stow (master) $ stow --adopt .
    rightbear@Rightbear:~/dotfiles_stow (master) $ cd $HOME
    rightbear@Rightbear:~ $ ls -lah .bash_aliases
    lrwxrwxrwx 1 rightbear rightbear 27 Jun 30 14:09 .bash_aliases -> dotfiles_stow/.bash_aliases
    rightbear@Rightbear:~ $ source .bashrc
    rightbear@Rightbear:~ $ alias -p

    alias alert='notify-send --urgency=low -i "$([ $? = 0 ] && echo terminal || echo error)" "$(history|tail -n1|sed -e '\''s/^\s*[0-9]\+\s*//;s/[;&|]\s*alert$//'\'')"'
    alias cl='clear'
    alias egrep='egrep --color=auto'
    alias et='exit'
    alias fgrep='fgrep --color=auto'
    alias grep='grep --color=auto'
    alias l='ls -CF'
    alias la='ls -A'
    alias ll='ls -alF'
    alias ls='ls --color=auto'
    alias lshome='ls -ltr ~'
    alias lstp='ls -ltr /tmp'
    …
    ```

6. Test your installation script on a fresh virtual machine.

    ## **Answer**
    You can finish Practice 7 & 8 first before running this part.

    ### Demo1 (Test with ~/dotfiles)
    ```console
    connlabtest@missing-semester-test:~$ ls -lahtr
    total 56K
    -rw-r--r--  1 connlabtest connlabtest  807 Sep  6  2025 .profile
    -rw-r--r--  1 connlabtest connlabtest 3.5K Sep  6  2025 .bashrc
    -rw-r--r--  1 connlabtest connlabtest  220 Sep  6  2025 .bash_logout
    drwxr-xr-x  3 root            root            4.0K Mar 17 03:59 ..
    -rw-------  1 connlabtest connlabtest 7.1K Mar 18 09:18 .viminfo
    -rw-------  1 connlabtest connlabtest   20 Mar 19 07:27 .lesshst
    drwxr-xr-x 13 connlabtest connlabtest 4.0K Apr 30 09:35 mosh
    -rw-------  1 connlabtest connlabtest 5.4K Apr 30 09:45 .bash_history
    drwx------  2 connlabtest connlabtest 4.0K Jun 30 07:33 .ssh
    -rw-r--r--  1 connlabtest connlabtest    0 Jun 30 07:41 .bash_aliases
    -rw-r--r--  1 connlabtest connlabtest    0 Jun 30 07:41 .bash_profile
    -rw-r--r--  1 connlabtest connlabtest    0 Jun 30 07:41 .tmux.conf
    drwxr-xr-x  6 connlabtest connlabtest 4.0K Jun 30 07:41 .
    connlabtest@missing-semester-test:~$ git clone https://github.com/connlabtest-sys/dotfiles.git
    Cloning into 'dotfiles'...
    remote: Enumerating objects: 10, done.
    remote: Counting objects: 100% (10/10), done.
    remote: Compressing objects: 100% (8/8), done.
    remote: Total 10 (delta 2), reused 10 (delta 2), pack-reused 0 (from 0)
    Receiving objects: 100% (10/10), 4.57 KiB | 1.14 MiB/s, done.
    Resolving deltas: 100% (2/2), done.
    connlabtest@missing-semester-test:~$ ls dotfiles/
    bash_aliases  bash_profile  bashrc  makesymlinks.sh  tmux.conf
    connlabtest@missing-semester-test:~$ cd dotfiles/
    connlabtest@missing-semester-test:~/dotfiles$ chmod +x makesymlinks.sh
    connlabtest@missing-semester-test:~/dotfiles$ ./makesymlinks.sh
    Creating /home/connlabtest/dotfiles_old for backup of any existing dotfiles in ~ ...done
    Changing to the /home/connlabtest/dotfiles directory ...done
    Moving any existing dotfiles from ~ to /home/connlabtest/dotfiles_old
    Creating symlink to bash_aliases in home directory.
    Moving any existing dotfiles from ~ to /home/connlabtest/dotfiles_old
    Creating symlink to bash_profile in home directory.
    Moving any existing dotfiles from ~ to /home/connlabtest/dotfiles_old
    Creating symlink to bashrc in home directory.
    Moving any existing dotfiles from ~ to /home/connlabtest/dotfiles_old
    Creating symlink to tmux.conf in home directory.
    connlabtest@missing-semester-test:~/dotfiles$ cd $HOME
    connlabtest@missing-semester-test:~$ ls -lahtr
    total 52K
    -rw-r--r--  1 connlabtest connlabtest  807 Sep  6  2025 .profile
    -rw-r--r--  1 connlabtest connlabtest  220 Sep  6  2025 .bash_logout
    drwxr-xr-x  3 root            root            4.0K Mar 17 03:59 ..
    -rw-------  1 connlabtest connlabtest 7.1K Mar 18 09:18 .viminfo
    -rw-------  1 connlabtest connlabtest   20 Mar 19 07:27 .lesshst
    drwxr-xr-x 13 connlabtest connlabtest 4.0K Apr 30 09:35 mosh
    -rw-------  1 connlabtest connlabtest 5.4K Apr 30 09:45 .bash_history
    drwx------  2 connlabtest connlabtest 4.0K Jun 30 07:33 .ssh
    drwxr-xr-x  3 connlabtest connlabtest 4.0K Jun 30 07:34 dotfiles
    lrwxrwxrwx  1 connlabtest connlabtest   43 Jun 30 07:43 .bash_aliases -> /home/connlabtest/dotfiles/bash_aliases
    lrwxrwxrwx  1 connlabtest connlabtest   43 Jun 30 07:43 .bash_profile -> /home/connlabtest/dotfiles/bash_profile
    drwxr-xr-x  2 connlabtest connlabtest 4.0K Jun 30 07:43 dotfiles_old
    lrwxrwxrwx  1 connlabtest connlabtest   37 Jun 30 07:43 .bashrc -> /home/connlabtest/dotfiles/bashrc
    lrwxrwxrwx  1 connlabtest connlabtest   40 Jun 30 07:43 .tmux.conf -> /home/connlabtest/dotfiles/tmux.conf
    drwxr-xr-x  6 connlabtest connlabtest 4.0K Jun 30 07:43 .
    connlabtest@missing-semester-test:~$ ls -a dotfiles_old
    .  ..  .bash_aliases  .bash_profile  .bashrc  .tmux.conf
    connlabtest@missing-semester-test:~$ source .bashrc
    connlabtest@missing-semester-test:~ $ cd dotfiles
    connlabtest@missing-semester-test:~/dotfiles (main *) $ cd $HOME
    connlabtest@missing-semester-test:~ $ updateall
    Get:1 file:/etc/apt/mirrors/debian.list Mirrorlist [30 B]
    Get:3 file:/etc/apt/mirrors/debian-security.list Mirrorlist [39 B]                      
    Hit:7 https://packages.cloud.google.com/apt google-compute-engine-bookworm-stable InRelease
    Hit:2 https://deb.debian.org/debian bookworm InRelease       
    Get:4 https://deb.debian.org/debian bookworm-updates InRelease [55.4 kB]
    Get:5 https://deb.debian.org/debian bookworm-backports InRelease [59.4 kB]
    Hit:6 https://deb.debian.org/debian-security bookworm-security InRelease
    Hit:8 https://packages.cloud.google.com/apt cloud-sdk-bookworm InRelease
    Fetched 115 kB in 1s (141 kB/s)
    Reading package lists... Done
    Building dependency tree... Done
    Reading state information... Done
    5 packages can be upgraded. Run 'apt list --upgradable' to see them.
    ```

    ### Demo2 (Test with ~/dotfiles_stow)
    ```console
    connlabtest@missing-semester-test:~$ ls -lahtr
    total 48K
    -rw-r--r--  1 connlabtest connlabtest  807 Sep  6  2025 .profile
    -rw-r--r--  1 connlabtest connlabtest 3.5K Sep  6  2025 .bashrc
    -rw-r--r--  1 connlabtest connlabtest  220 Sep  6  2025 .bash_logout
    -rw-------  1 connlabtest connlabtest 7.1K Mar 18 09:18 .viminfo
    drwxr-xr-x 13 connlabtest connlabtest 4.0K Apr 30 09:35 mosh
    -rw-r--r--  1 connlabtest connlabtest    0 Jun 30 07:41 .bash_aliases
    -rw-r--r--  1 connlabtest connlabtest    0 Jun 30 07:41 .bash_profile
    -rw-r--r--  1 connlabtest connlabtest    0 Jun 30 07:41 .tmux.conf
    drwxr-xr-x  3 root            root            4.0K Jun 30 07:56 ..
    -rw-------  1 connlabtest connlabtest   20 Jun 30 07:57 .lesshst
    drwxr-xr-x  4 connlabtest connlabtest 4.0K Jun 30 07:57 .
    -rw-------  1 connlabtest connlabtest 6.5K Jun 30 07:57 .bash_history
    drwx------  2 connlabtest connlabtest 4.0K Jun 30 08:01 .ssh
    connlabtest@missing-semester-test:~$ mv .bashrc .bashrc.bkp
    connlabtest@missing-semester-test:~$ mv .bash_aliases .bash_aliases.bkp
    connlabtest@missing-semester-test:~$ mv .bash_profile .bash_profile.bkp
    connlabtest@missing-semester-test:~$ mv .tmux.conf .tmux.conf.bk
    connlabtest@missing-semester-test:~$ git clone https://github.com/connlabtest-sys/dotfiles_stow.git
    Cloning into 'dotfiles_stow'...
    remote: Enumerating objects: 8, done.
    remote: Counting objects: 100% (8/8), done.
    remote: Compressing objects: 100% (6/6), done.
    remote: Total 8 (delta 1), reused 8 (delta 1), pack-reused 0 (from 0)
    Receiving objects: 100% (8/8), 4.11 KiB | 75.00 KiB/s, done.
    Resolving deltas: 100% (1/1), done.
    connlabtest@missing-semester-test:~$ cd dotfiles_stow
    connlabtest@missing-semester-test:~/dotfiles_stow$ stow .
    connlabtest@missing-semester-test:~/dotfiles_stow$ cd $HOME
    connlabtest@missing-semester-test:~$ ls -lahtr
    total 52K
    -rw-r--r--  1 connlabtest connlabtest  807 Sep  6  2025 .profile
    -rw-r--r--  1 connlabtest connlabtest 3.5K Sep  6  2025 .bashrc.bkp
    -rw-r--r--  1 connlabtest connlabtest  220 Sep  6  2025 .bash_logout
    -rw-------  1 connlabtest connlabtest 7.1K Mar 18 09:18 .viminfo
    drwxr-xr-x 13 connlabtest connlabtest 4.0K Apr 30 09:35 mosh
    -rw-r--r--  1 connlabtest connlabtest    0 Jun 30 07:41 .bash_aliases.bkp
    -rw-r--r--  1 connlabtest connlabtest    0 Jun 30 07:41 .bash_profile.bkp
    -rw-r--r--  1 connlabtest connlabtest    0 Jun 30 07:41 .tmux.conf.bkp
    drwxr-xr-x  3 root            root            4.0K Jun 30 07:56 ..
    -rw-------  1 connlabtest connlabtest   20 Jun 30 07:57 .lesshst
    -rw-------  1 connlabtest connlabtest 6.5K Jun 30 07:57 .bash_history
    drwx------  2 connlabtest connlabtest 4.0K Jun 30 08:01 .ssh
    drwxr-xr-x  3 connlabtest connlabtest 4.0K Jun 30 08:05 dotfiles_stow
    lrwxrwxrwx  1 connlabtest connlabtest   24 Jun 30 08:07 .tmux.conf -> dotfiles_stow/.tmux.conf
    lrwxrwxrwx  1 connlabtest connlabtest   21 Jun 30 08:07 .bashrc -> dotfiles_stow/.bashrc
    lrwxrwxrwx  1 connlabtest connlabtest   27 Jun 30 08:07 .bash_profile -> dotfiles_stow/.bash_profile
    lrwxrwxrwx  1 connlabtest connlabtest   27 Jun 30 08:07 .bash_aliases -> dotfiles_stow/.bash_aliases
    drwxr-xr-x  5 connlabtest connlabtest 4.0K Jun 30 08:07 .
    connlabtest@missing-semester-test:~$ source .bashrc
    connlabtest@missing-semester-test:~ $ cd dotfiles_stow/
    connlabtest@missing-semester-test:~/dotfiles_stow (main) $ cd $HOME
    connlabtest@missing-semester-test:~ $ updateall
    Get:1 file:/etc/apt/mirrors/debian.list Mirrorlist [30 B]
    Get:2 file:/etc/apt/mirrors/debian-security.list Mirrorlist [39 B]
    Hit:3 https://deb.debian.org/debian bookworm InRelease        
    Hit:4 https://deb.debian.org/debian bookworm-updates InRelease
    Hit:5 https://deb.debian.org/debian bookworm-backports InRelease
    Hit:7 https://packages.cloud.google.com/apt google-compute-engine-bookworm-stable InRelease
    Hit:6 https://deb.debian.org/debian-security bookworm-security InRelease
    Hit:8 https://packages.cloud.google.com/apt cloud-sdk-bookworm InRelease
    Reading package lists... Done
    Building dependency tree... Done
    Reading state information... Done
    5 packages can be upgraded. Run 'apt list --upgradable' to see them.
    ```

7. Migrate all of your current tool configurations to your dotfiles repository.

    ## **Answer**
    ### Demo1 (Test with ~/dotfiles)
    ```console
    rightbear@Rightbear:~ $ cp ~/.tmux.conf ~/dotfiles/tmux.conf
    rightbear@Rightbear:~ $ cp ~/.bashrc ~/dotfiles/bashrc
    rightbear@Rightbear:~ $ cp ~/.bash_profile ~/dotfiles/bash_profile
    rightbear@Rightbear:~ $ ls ~/dotfiles
    bash_aliases  bash_profile  bashrc  makesymlinks.sh  tmux.conf
    rightbear@Rightbear:~ $ cd ~/dotfiles
    rightbear@Rightbear:~/dotfiles (master %) $ vim makesymlinks.sh
    rightbear@Rightbear:~/dotfiles (master *%) $ git diff
    diff --git a/makesymlinks.sh b/makesymlinks.sh
    index b560a04..673e655 100644
    --- a/makesymlinks.sh
    +++ b/makesymlinks.sh
    @@ -11,7 +11,7 @@

    dir=~/dotfiles                    # dotfiles directory
    olddir=~/dotfiles_old             # old dotfiles backup directory
    -files="bash_aliases"    # list of files/folders to symlink in homedir
    +files="bash_aliases bash_profile bashrc tmux.conf"    # list of files/folders to symlink in homedir

    ##########

    rightbear@Rightbear:~/dotfiles (master *%) $ git add .
    rightbear@Rightbear:~/dotfiles (master +) $ git commit -m "Migrate all of the current tool configurations"
    [master a32e054] Migrate all of the current tool configurations
    4 files changed, 403 insertions(+), 1 deletion(-)
    create mode 100644 bash_profile
    create mode 100644 bashrc
    create mode 100644 tmux.conf
    rightbear@Rightbear:~/dotfiles (master) $ git log
    commit a32e054b88608b410fd4cee8eadeaa338963cce0 (HEAD -> master)
    Author: connlabtest <connlabtest@gmail.com>
    Date:   Tue Jun 30 14:30:40 2026 +0800

        Migrate all of the current tool configurations

    commit 30008b1ea832b48b129edb0dd4c68a55bf672204
    Author: connlabtest <connlabtest@gmail.com>
    Date:   Mon Jun 29 15:40:07 2026 +0800

        My first Git commit of my dotfiles

    ```

    ### Demo2 (Test with ~/dotfiles_stow)
    ```console
    rightbear@Rightbear:~ $ cp ~/.tmux.conf ~/dotfiles_stow/.tmux.conf
    rightbear@Rightbear:~ $ cp ~/.bashrc ~/dotfiles_stow/.bashrc
    rightbear@Rightbear:~ $ cp ~/.bash_profile ~/dotfiles_stow/.bash_profile
    rightbear@Rightbear:~ $ ls -a ~/dotfiles_stow/
    .  ..  .bash_aliases  .bash_profile  .bashrc  .git  .tmux.conf
    rightbear@Rightbear:~ $ cd ~/dotfiles_stow/
    rightbear@Rightbear:~/dotfiles_stow (master %) $ git add .
    rightbear@Rightbear:~/dotfiles_stow (master +) $ git commit -m "Migrate all of the current tool configurations with GNU Stow"
    [master b6ece49] Migrate all of the current tool configurations with GNU Stow
    3 files changed, 402 insertions(+)
    create mode 100644 .bash_profile
    create mode 100644 .bashrc
    create mode 100644 .tmux.conf
    rightbear@Rightbear:~/dotfiles_stow (master) $ git log
    commit b6ece49ac51e6653933117a5f1c01f995b0ee85b (HEAD -> master)
    Author: connlabtest <connlabtest@gmail.com>
    Date:   Tue Jun 30 14:40:43 2026 +0800

        Migrate all of the current tool configurations with GNU Stow

    commit 8a9d0c3908b30fa4188301336d661c76751045b0
    Author: connlabtest <connlabtest@gmail.com>
    Date:   Tue Jun 30 13:48:53 2026 +0800

        My first Git commit of my dotfiles with GNU Stow

    ```

8. Publish your dotfiles on GitHub.

    ## **Answer**
    ### Demo1 (Test with ~/dotfiles)
    ```console
    rightbear@Rightbear:~ $ cd dotfiles
    rightbear@Rightbear:~/dotfiles (master) $ git remote add origin git@github.com:connlabtest-sys/dotfiles.git
    rightbear@Rightbear:~/dotfiles (master) $ git branch -M main
    rightbear@Rightbear:~/dotfiles (main) $ git push -u origin main
    Enumerating objects: 10, done.
    Counting objects: 100% (10/10), done.
    Delta compression using up to 12 threads
    Compressing objects: 100% (10/10), done.
    Writing objects: 100% (10/10), 4.57 KiB | 780.00 KiB/s, done.
    Total 10 (delta 2), reused 0 (delta 0), pack-reused 0
    remote: Resolving deltas: 100% (2/2), done.
    To github.com:connlabtest-sys/dotfiles.git
    * [new branch]      main -> main
    branch 'main' set up to track 'origin/main'.
    rightbear@Rightbear:~/dotfiles (main) $ git log
    commit a32e054b88608b410fd4cee8eadeaa338963cce0 (HEAD -> main, origin/main)
    Author: connlabtest <connlabtest@gmail.com>
    Date:   Tue Jun 30 14:30:40 2026 +0800

        Migrate all of the current tool configurations

    commit 30008b1ea832b48b129edb0dd4c68a55bf672204
    Author: connlabtest <connlabtest@gmail.com>
    Date:   Mon Jun 29 15:40:07 2026 +0800

        My first Git commit of my dotfiles

    ```

    ### Demo2 (Test with ~/dotfiles_stow)
    ```console
    rightbear@Rightbear:~ $ cd dotfiles_stow/
    rightbear@Rightbear:~/dotfiles_stow (master) $ git remote add origin git@github.com:connlabtest-sys/dotfiles_stow.git
    rightbear@Rightbear:~/dotfiles_stow (master) $ git branch -M main
    rightbear@Rightbear:~/dotfiles_stow (main) $ git push -u origin main
    Enumerating objects: 8, done.
    Counting objects: 100% (8/8), done.
    Delta compression using up to 12 threads
    Compressing objects: 100% (7/7), done.
    Writing objects: 100% (8/8), 4.11 KiB | 2.05 MiB/s, done.
    Total 8 (delta 1), reused 0 (delta 0), pack-reused 0
    remote: Resolving deltas: 100% (1/1), done.
    To github.com:connlabtest-sys/dotfiles_stow.git
    * [new branch]      main -> main
    branch 'main' set up to track 'origin/main'.
    rightbear@Rightbear:~/dotfiles_stow (main) $ git log
    commit b6ece49ac51e6653933117a5f1c01f995b0ee85b (HEAD -> main, origin/main)
    Author: connlabtest <connlabtest@gmail.com>
    Date:   Tue Jun 30 14:40:43 2026 +0800

        Migrate all of the current tool configurations with GNU Stow

    commit 8a9d0c3908b30fa4188301336d661c76751045b0
    Author: connlabtest <connlabtest@gmail.com>
    Date:   Tue Jun 30 13:48:53 2026 +0800

        My first Git commit of my dotfiles with GNU Stow

    ```

## Remote Machines (SSH)

Install a Linux virtual machine (or use an already existing one) for these exercises. If you are not familiar with virtual machines check out [this](https://hibbard.eu/install-ubuntu-virtual-box/) tutorial for installing one.

1. Go to `~/.ssh/` and check if you have a pair of SSH keys there. If not, generate them with `ssh-keygen -a 100 -t ed25519`. It is recommended that you use a password and use `ssh-agent`, more info [here](https://www.ssh.com/ssh/agent).

    ## **Answer**
    ### Demo
    ```console
    rightbear@Rightbear:~ $ cd .ssh
    rightbear@Rightbear:~/.ssh $ ls -lahtr
    total 12K
    -rw-r--r-- 1 rightbear rightbear  142 Jun 30 07:33 known_hosts
    drwx------ 2 rightbear rightbear 4.0K Jul  1 02:42 .
    drwxr-xr-x 6 rightbear rightbear 4.0K Jul  1 02:53 ..
    rightbear@Rightbear:~/.ssh $ ssh-keygen -a 100 -t ed25519
    Generating public/private ed25519 key pair.
    Enter file in which to save the key (/home/rightbear/.ssh/id_ed25519): 
    Enter passphrase (empty for no passphrase): 
    Enter same passphrase again: 
    Your identification has been saved in /home/rightbear/.ssh/id_ed25519
    Your public key has been saved in /home/rightbear/.ssh/id_ed25519.pub
    The key fingerprint is:
    SHA256:xf12CIk5txEA5ayADi58htwZTdAB2P8fZhfI24iRhXo rightbear@Rightbear
    The key's randomart image is:
    +--[ED25519 256]--+
    |  oo+o. ooo..    |
    | . .oo . = + o   |
    |  ..o.o + X *    |
    |o.oooo E = + = . |
    |.+.=. o S + o + .|
    | .o    o * o . . |
    |        + o      |
    |         .       |
    |                 |
    +----[SHA256]-----+

    rightbear@Rightbear:~/.ssh $ ls -lahtr
    total 20K
    -rw-r--r-- 1 rightbear rightbear  142 Jun 30 07:33 known_hosts
    drwxr-xr-x 6 rightbear rightbear 4.0K Jul  1 02:53 ..
    -rw-r--r-- 1 rightbear rightbear  119 Jul  1 02:59 id_ed25519.pub
    -rw------- 1 rightbear rightbear  484 Jul  1 02:59 id_ed25519
    drwx------ 2 rightbear rightbear 4.0K Jul  1 02:59 .
    rightbear@Rightbear:~/.ssh $ ssh-agent
    SSH_AUTH_SOCK=/tmp/ssh-K6yAje1A6Kc0/agent.2237434; export SSH_AUTH_SOCK;
    SSH_AGENT_PID=2237435; export SSH_AGENT_PID;
    echo Agent pid 2237435;
    rightbear@Rightbear:~/.ssh $ eval `ssh-agent`
    Agent pid 2237452
    rightbear@Rightbear:~/.ssh $ ssh-add
    Enter passphrase for /home/rightbear/.ssh/id_ed25519: 
    Identity added: /home/rightbear/.ssh/id_ed25519 (rightbear@Rightbear)
    rightbear@Rightbear:~/.ssh $ ssh-add -l
    256 SHA256:xf12CIk5txEA5ayADi58htwZTdAB2P8fZhfI24iRhXo rightbear@Rightbear (ED25519)
    ```

2. Edit `.ssh/config` to have an entry as follows:

    ```bash
    Host vm
        User username_goes_here
        HostName ip_goes_here
        IdentityFile ~/.ssh/id_ed25519
        LocalForward 9999 localhost:8888
    ```

    ## **Answer**
    ### Configuration file (~/.ssh/config)
    ```bash
    Host missing-semester-test
        User connlabtest
        HostName 35.212.166.196
        IdentityFile ~/.ssh/id_ed25519
        LocalForward 9999 localhost:8888
    ```

    ### Demo
    ```console
    rightbear@Rightbear:~ $ vim .ssh/config
    ```

3. Use `ssh-copy-id vm` to copy your ssh key to the server.

    ## **Answer**
    The password you set in SSH Server should be the same as the password you input in SSH Client.

    ### Demo1 (SSH Server)
    ```console
    connlabtest@missing-semester-test:~/.ssh $ sudo passwd connlabtest
    New password: 
    Retype new password: 
    passwd: password updated successfully
    connlabtest@missing-semester-test:~ $ cd $HOME
    connlabtest@missing-semester-test:~ $ ls -a
    .              .bash_aliases.bkp  .bash_profile      .bashrc.bkp  .ssh            .viminfo      mosh
    ..             .bash_history      .bash_profile.bkp  .lesshst     .tmux.conf      dotfiles
    .bash_aliases  .bash_logout       .bashrc            .profile     .tmux.conf.bkp  dotfiles_old
    ```

    ### Demo2 (SSH Client)
    ```console
    rightbear@Rightbear:~ $ ssh-copy-id missing-semester-test
    /usr/bin/ssh-copy-id: INFO: Source of key(s) to be installed: "/home/rightbear/.ssh/id_ed25519.pub"
    /usr/bin/ssh-copy-id: INFO: attempting to log in with the new key(s), to filter out any that are already installed
    /usr/bin/ssh-copy-id: INFO: 1 key(s) remain to be installed -- if you are prompted now it is to install the new keys
    connlabtest@35.212.166.196's password:

    Number of key(s) added: 1

    Now try logging into the machine, with:   "ssh 'missing-semester-test'"
    and check to make sure that only the key(s) you wanted were added.

    rightbear@Rightbear:~ $ ssh missing-semester-test
    Linux missing-semester-test 6.1.0-43-cloud-amd64 #1 SMP PREEMPT_DYNAMIC Debian 6.1.162-1 (2026-02-08) x86_64

    The programs included with the Debian GNU/Linux system are free software;
    the exact distribution terms for each program are described in the
    individual files in /usr/share/doc/*/copyright.

    Debian GNU/Linux comes with ABSOLUTELY NO WARRANTY, to the extent
    permitted by applicable law.
    Last login: Wed Jul  1 02:39:35 2026 from 35.235.240.144
    connlabtest@missing-semester-test:~ $ ls -a
    .              .bash_aliases.bkp  .bash_profile      .bashrc.bkp  .ssh            .viminfo      mosh
    ..             .bash_history      .bash_profile.bkp  .lesshst     .tmux.conf      dotfiles
    .bash_aliases  .bash_logout       .bashrc            .profile     .tmux.conf.bkp  dotfiles_old
    ```

4. Start a webserver in your VM by executing `python -m http.server 8888`. Access the VM webserver by navigating to `http://localhost:9999` in your machine.

    ## **Answer**
    ### Demo1 (SSH Server)
    ```console
    connlabtest@missing-semester-test:~ $ alias python=python3
    connlabtest@missing-semester-test:~ $ python -m http.server 8888
    Serving HTTP on 0.0.0.0 port 8888 (http://0.0.0.0:8888/) ...
    ```
    
    ### Demo2 (SSH Client Terminal 1)
    Launch the first terminal of SSH Client to build a SSH connection with SSH Server from SSH Client.
    ```console
    rightbear@Rightbear:~ $ ssh missing-semester-test
    connlabtest@35.212.166.196's password:
    Linux missing-semester-test 6.1.0-43-cloud-amd64 #1 SMP PREEMPT_DYNAMIC Debian 6.1.162-1 (2026-02-08) x86_64

    The programs included with the Debian GNU/Linux system are free software;
    the exact distribution terms for each program are described in the
    individual files in /usr/share/doc/*/copyright.

    Debian GNU/Linux comes with ABSOLUTELY NO WARRANTY, to the extent
    permitted by applicable law.
    Last login: Wed Jul  1 05:33:32 2026 from 39.12.129.116
    connlabtest@missing-semester-test:~ $
    ```

    ### Demo3 (SSH Client Terminal 2)
    Launch the second terminal of SSH Client to send HTTP requests from SSH Client to SSH Server.
    ```console
    rightbear@Rightbear:~ $ curl http://localhost:9999
    <!DOCTYPE HTML>
    <html lang="en">
    <head>
    <meta charset="utf-8">
    <title>Directory listing for /</title>
    </head>
    <body>
    <h1>Directory listing for /</h1>
    <hr>
    <ul>
    <li><a href=".bash_aliases">.bash_aliases@</a></li>
    <li><a href=".bash_aliases.bkp">.bash_aliases.bkp</a></li>
    <li><a href=".bash_history">.bash_history</a></li>
    <li><a href=".bash_logout">.bash_logout</a></li>
    <li><a href=".bash_profile">.bash_profile@</a></li>
    <li><a href=".bash_profile.bkp">.bash_profile.bkp</a></li>
    <li><a href=".bashrc">.bashrc@</a></li>
    <li><a href=".bashrc.bkp">.bashrc.bkp</a></li>
    <li><a href=".lesshst">.lesshst</a></li>
    <li><a href=".profile">.profile</a></li>
    <li><a href=".ssh/">.ssh/</a></li>
    <li><a href=".tmux.conf">.tmux.conf@</a></li>
    <li><a href=".tmux.conf.bkp">.tmux.conf.bkp</a></li>
    <li><a href=".viminfo">.viminfo</a></li>
    <li><a href="dotfiles/">dotfiles/</a></li>
    <li><a href="dotfiles_old/">dotfiles_old/</a></li>
    <li><a href="mosh/">mosh/</a></li>
    </ul>
    <hr>
    </body>
    </html>
    rightbear@Rightbear:~ $ curl -I http://localhost:9999
    HTTP/1.0 200 OK
    Server: SimpleHTTP/0.6 Python/3.11.2
    Date: Wed, 01 Jul 2026 05:53:59 GMT
    Content-type: text/html; charset=utf-8
    Content-Length: 982

    ```

    ### Demo4 (SSH Server)
    SSH Server will receive GET and HEAD results from SSH Client Terminal 2. The timestamp should be the same for the information of HTTP requests on both sides, which is `01/Jul/2026 05:42` here. 
    ```console
    connlabtest@missing-semester-test:~ $ python -m http.server 8888
    Serving HTTP on 0.0.0.0 port 8888 (http://0.0.0.0:8888/) ...
    127.0.0.1 - - [01/Jul/2026 05:53:48] "GET / HTTP/1.1" 200 -
    127.0.0.1 - - [01/Jul/2026 05:53:59] "HEAD / HTTP/1.1" 200 - 
    ```

5. Edit your SSH server config by doing `sudo vim /etc/ssh/sshd_config` and disable password authentication by editing the value of `PasswordAuthentication`. Disable root login by editing the value of `PermitRootLogin`. Restart the `ssh` service with `sudo service sshd restart`. Try sshing in again.

    ## **Answer**
    ### Demo1 (SSH Server)
    ```console
    connlabtest@missing-semester-test:~ $ sudo vim /etc/ssh/sshd_config
    …
    # To disable tunneled clear text passwords, change to no here!
    PasswordAuthentication no
    #PermitEmptyPasswords no
    …
    # PAM authentication via KbdInteractiveAuthentication may bypass
    PermitRootLogin no
    # If you just want the PAM account and session checks to run without
    # PAM authentication, then enable this but set PasswordAuthentication
    # and KbdInteractiveAuthentication to 'no'.
    …
    connlabtest@missing-semester-test:~ $ sudo service sshd restart
    ```

    ### Demo2 (SSH Client)
    ```console
    rightbear@Rightbear:~ $ ssh missing-semester-test
    connlabtest@35.212.166.196: Permission denied (publickey).
    ```

6. (Challenge) Install [`mosh`](https://mosh.org/) in the VM and establish a connection. Then disconnect the network adapter of the server/VM. Can mosh properly recover from it?

    ## **Answer**
    On the firewall of the Mosh server, make sure you open up UDP ports 60000-61000 for the ingress traffic.
    
    ### Demo1 (Mosh Server)
    ```console
    connlabtest@missing-semester-test:~ $ mosh-server


    MOSH CONNECT 60001 Mg899P23wEouyjF+sia3Vg

    mosh-server (mosh 1.4.0) [build mosh-1.4.0-39-gdecd9b7]
    Copyright 2012 Keith Winstein <mosh-devel@mit.edu>
    License GPLv3+: GNU GPL version 3 or later <http://gnu.org/licenses/gpl.html>.
    This is free software: you are free to change and redistribute it.
    There is NO WARRANTY, to the extent permitted by law.

    [mosh-server detached, pid = 2241671]
    ```

    ### Demo2 (Mosh Client)
    ```console
    rightbear@Rightbear:~ $ mosh missing-semester-test
    connlabtest@35.212.166.196's password:

    Linux missing-semester-test 6.1.0-43-cloud-amd64 #1 SMP PREEMPT_DYNAMIC Debian 6.1.162-1 (2026-02-08) x86_64

    The programs included with the Debian GNU/Linux system are free software;
    the exact distribution terms for each program are described in the
    individual files in /usr/share/doc/*/copyright.

    Debian GNU/Linux comes with ABSOLUTELY NO WARRANTY, to the extent
    permitted by applicable law.
    connlabtest@missing-semester-test:~ $
    ```

    ### Demo3 (Mosh Server & Mosh Client)
    Make the network interface on the Mosh Server side down for 15 seconds, then restart the network interface.
    ```console
    connlabtest@missing-semester-test:~ $ ip link
    1: lo: <LOOPBACK,UP,LOWER_UP> mtu 65536 qdisc noqueue state UNKNOWN mode DEFAULT group default qlen 1000
        link/loopback 00:00:00:00:00:00 brd 00:00:00:00:00:00
    2: ens4: <BROADCAST,MULTICAST,UP,LOWER_UP> mtu 1460 qdisc mq state UP mode DEFAULT group default qlen 1000
        link/ether 42:01:0a:8a:00:02 brd ff:ff:ff:ff:ff:ff
        altname enp0s4
    connlabtest@missing-semester-test:~ $ sudo ip link set ens4 down && sleep 15 && sudo ip link set ens4 up
    ```

    While the network interface on the Mosh server side is down for 15 seconds, the Mosh client will display a warning message and the time will keep increasing.
    ```console
    mosh: Last contact 15 seconds ago. [To quit: Ctrl-^ .]
    x86_64

    The programs included with the Debian GNU/Linux system are free software;
    the exact distribution terms for each program are described in the
    individual files in /usr/share/doc/*/copyright.

    Debian GNU/Linux comes with ABSOLUTELY NO WARRANTY, to the extent
    permitted by applicable law.
    connlabtest@missing-semester-test:~ $
    connlabtest@missing-semester-test:~ $ ls

    ```

    After the 15 seconds are up, the network interface on the Mosh server side will recover, and the warning message on the Mosh client side will disappear. Meanwhile, the connection between the Mosh server and client remains active throughout the process.
    ```console
    Linux missing-semester-test 6.1.0-43-cloud-amd64 #1 SMP PREEMPT_DYNAMIC Debian 6.1.162-1 (2026-02-08) x86_64

    The programs included with the Debian GNU/Linux system are free software;
    the exact distribution terms for each program are described in the
    individual files in /usr/share/doc/*/copyright.

    Debian GNU/Linux comes with ABSOLUTELY NO WARRANTY, to the extent
    permitted by applicable law.
    connlabtest@missing-semester-test:~ $
    connlabtest@missing-semester-test:~ $ ls -a
    .              .bash_aliases.bkp  .bash_profile      .bashrc.bkp  .ssh            .viminfo      mosh
    ..             .bash_history      .bash_profile.bkp  .lesshst     .tmux.conf      dotfiles
    .bash_aliases  .bash_logout       .bashrc            .profile     .tmux.conf.bkp  dotfiles_old
    ```

7. (Challenge) Look into what the `-N` and `-f` flags do in `ssh` and figure out a command to achieve background port forwarding.

    ## **Answer**
    ### Explanation
    `-f` flag in ssh requests ssh to go to background just before command execution. This is useful if ssh is going to ask for passwords or passphrases, but the user wants it in the background. 
    `-N` flag in ssh does not execute a remote command. This is useful for just forwarding ports.
    The combination of two flags can make scenarios in Practice 4 of this chapter much simpler to test local port forwarding. When the flags are used together (e.g., `ssh -Nf` ...) , users can run the SSH forwarding task quietly in the background. Accordingly, the SSH Client side only needs to open one terminal to finish two tasks, which are connecting to the SSH Server side in the background and running the curl command to access the webserver.

    ### Demo1 (SSH Server)
    ```console
    connlabtest@missing-semester-test:~ $ alias python=python3
    connlabtest@missing-semester-test:~ $ python -m http.server 8888
    Serving HTTP on 0.0.0.0 port 8888 (http://0.0.0.0:8888/) ...
    ```

    ### Demo2 (SSH Client)
    Launch the terminal of SSH Client to build a SSH connection with SSH Server from SSH Client, then you can send HTTP requests from SSH Client to SSH Server. No need to switch to another terminal.
    ```console
    rightbear@Rightbear:~ $ ssh -Nf missing-semester-test
    connlabtest@35.212.166.196's password:
    rightbear@Rightbear:~ $ curl http://localhost:9999
    <!DOCTYPE HTML>
    <html lang="en">
    <head>
    <meta charset="utf-8">
    <title>Directory listing for /</title>
    </head>
    <body>
    <h1>Directory listing for /</h1>
    <hr>
    <ul>
    <li><a href=".bash_aliases">.bash_aliases@</a></li>
    <li><a href=".bash_aliases.bkp">.bash_aliases.bkp</a></li>
    <li><a href=".bash_history">.bash_history</a></li>
    <li><a href=".bash_logout">.bash_logout</a></li>
    <li><a href=".bash_profile">.bash_profile@</a></li>
    <li><a href=".bash_profile.bkp">.bash_profile.bkp</a></li>
    <li><a href=".bashrc">.bashrc@</a></li>
    <li><a href=".bashrc.bkp">.bashrc.bkp</a></li>
    <li><a href=".lesshst">.lesshst</a></li>
    <li><a href=".profile">.profile</a></li>
    <li><a href=".ssh/">.ssh/</a></li>
    <li><a href=".tmux.conf">.tmux.conf@</a></li>
    <li><a href=".tmux.conf.bkp">.tmux.conf.bkp</a></li>
    <li><a href=".viminfo">.viminfo</a></li>
    <li><a href="dotfiles/">dotfiles/</a></li>
    <li><a href="dotfiles_old/">dotfiles_old/</a></li>
    <li><a href="mosh/">mosh/</a></li>
    </ul>
    <hr>
    </body>
    </html>
    rightbear@Rightbear:~ $ curl -I http://localhost:9999
    HTTP/1.0 200 OK
    Server: SimpleHTTP/0.6 Python/3.11.2
    Date: Thu, 02 Jul 2026 07:41:51 GMT
    Content-type: text/html; charset=utf-8
    Content-Length: 982

    ```

    ### Demo3 (SSH Server)
    SSH Server will receive GET and HEAD results from SSH Client Terminal. The timestamp should be the same for the information of HTTP requests on both sides, which is `02/Jul/2026 07:41` here. 
    ```console
    connlabtest@missing-semester-test:~ $ python -m http.server 8888
    Serving HTTP on 0.0.0.0 port 8888 (http://0.0.0.0:8888/) ...
    127.0.0.1 - - [02/Jul/2026 07:41:35] "GET / HTTP/1.1" 200 -
    127.0.0.1 - - [02/Jul/2026 07:41:51] "HEAD / HTTP/1.1" 200 -
    ```

    ### Demo4 (SSH Client)
    If you want to kill the ssh connection built in the background, you can use `ps aux` and `kill` commands to kill the process.
    ```console
    rightbear@Rightbear:~ $
    rightbear@Rightbear:~ $ ps aux | grep ssh
    rightbear     2677  0.0  0.0   8436  2996 ?        Ss   05:30   0:00 ssh-agent
    rightbear     2683  0.0  0.0   8304  1460 ?        Ss   05:30   0:00 ssh-agent
    rightbear     7160  0.0  0.0  12016  5236 ?        Ss   15:39   0:00 ssh -Nf missing-semester-test
    rightbear     7181 33.3  0.0   4088  1920 pts/0    S+   15:45   0:00 grep --color=auto ssh
    rightbear@Rightbear:~ $ kill 7160
    rightbear@Rightbear:~ $ ps aux | grep ssh
    rightbear     2677  0.0  0.0   8436  2996 ?        Ss   05:30   0:00 ssh-agent
    rightbear     2683  0.0  0.0   8304  1460 ?        Ss   05:30   0:00 ssh-agent
    rightbear     7191  0.0  0.0   4088  1920 pts/0    S+   15:46   0:00 grep --color=auto ssh
    ```