# Exercises

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

{% comment %}
ls -lath --color=auto
{% endcomment %}

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
    `printenv` command outputs environment variables in a "raw" format: `KEY=VALUE`. (e.g. `HOME=/home/rightbear`)
    `export` command, as a Shell built-in, outputs environment variables in a format that the Shell can re-execute as a script. It adds the prefix `declare -x` and wraps values in double quotes. (e.g. `declare -x HOME="/home/rightbear"`)
    Because `diff` command is a literal line-by-line comparison tool, it sees `HOME=...` and `declare -x HOME="..."` as completely different strings.

## Environment Variables

1. Write bash functions `marco` and `polo` that do the following: whenever you execute `marco` the current working directory should be saved in some manner, then when you execute `polo`, no matter what directory you are in, `polo` should `cd` you back to the directory where you executed `marco`. For ease of debugging you can write the code in a file `marco.sh` and (re)load the definitions to your shell by executing `source marco.sh`.

{% comment %}
marco() {
    export MARCO=$(pwd)
}

polo() {
    cd "$MARCO"
}
{% endcomment %}

## Return Codes

1. Say you have a command that fails rarely. In order to debug it you need to capture its output but it can be time consuming to get a failure run. Write a bash script that runs the following script until it fails and captures its standard output and error streams to files and prints everything at the end. Bonus points if you can also report how many runs it took for the script to fail.

    ```bash
    #!/usr/bin/env bash

    n=$(( RANDOM % 100 ))

    if [[ n -eq 42 ]]; then
       echo "Something went wrong"
       >&2 echo "The error was using magic numbers"
       exit 1
    fi

    echo "Everything went according to plan"
    ```

{% comment %}
#!/usr/bin/env bash

count=0
until [[ "$?" -ne 0 ]];
do
  count=$((count+1))
  ./random.sh &> out.txt
done

echo "found error after $count runs"
cat out.txt
{% endcomment %}

## Signals and Job Control

1. Start a `sleep 10000` job in a terminal, background it with `Ctrl-Z` and continue its execution with `bg`. Now use [`pgrep`](https://www.man7.org/linux/man-pages/man1/pgrep.1.html) to find its pid and [`pkill`](https://man7.org/linux/man-pages/man1/pgrep.1.html) to kill it without ever typing the pid itself. (Hint: use the `-af` flags).

2. Say you don't want to start a process until another completes. How would you go about it? In this exercise, our limiting process will always be `sleep 60 &`. One way to achieve this is to use the [`wait`](https://www.man7.org/linux/man-pages/man1/wait.1p.html) command. Try launching the sleep command and having an `ls` wait until the background process finishes.

    However, this strategy will fail if we start in a different bash session, since `wait` only works for child processes. One feature we did not discuss in the notes is that the `kill` command's exit status will be zero on success and nonzero otherwise. `kill -0` does not send a signal but will give a nonzero exit status if the process does not exist. Write a bash function called `pidwait` that takes a pid and waits until the given process completes. You should use `sleep` to avoid wasting CPU unnecessarily.

## Files and Permissions

1. (Advanced) Write a command or script to recursively find the most recently modified file in a directory. More generally, can you list all files by recency?

## Terminal Multiplexers

1. Follow this `tmux` [tutorial](https://www.hamvocke.com/blog/a-quick-and-easy-guide-to-tmux/) and then learn how to do some basic customizations following [these steps](https://www.hamvocke.com/blog/a-guide-to-customizing-your-tmux-conf/).

## Aliases and Dotfiles

1. Create an alias `dc` that resolves to `cd` for when you type it wrong.

2. Run `history | awk '{$1="";print substr($0,2)}' | sort | uniq -c | sort -n | tail -n 10` to get your top 10 most used commands and consider writing shorter aliases for them. Note: this works for Bash; if you're using ZSH, use `history 1` instead of just `history`.

3. Create a folder for your dotfiles and set up version control.

4. Add a configuration for at least one program, e.g. your shell, with some customization (to start off, it can be something as simple as customizing your shell prompt by setting `$PS1`).

5. Set up a method to install your dotfiles quickly (and without manual effort) on a new machine. This can be as simple as a shell script that calls `ln -s` for each file, or you could use a [specialized utility](https://dotfiles.github.io/utilities/).

6. Test your installation script on a fresh virtual machine.

7. Migrate all of your current tool configurations to your dotfiles repository.

8. Publish your dotfiles on GitHub.

## Remote Machines (SSH)

Install a Linux virtual machine (or use an already existing one) for these exercises. If you are not familiar with virtual machines check out [this](https://hibbard.eu/install-ubuntu-virtual-box/) tutorial for installing one.

1. Go to `~/.ssh/` and check if you have a pair of SSH keys there. If not, generate them with `ssh-keygen -a 100 -t ed25519`. It is recommended that you use a password and use `ssh-agent`, more info [here](https://www.ssh.com/ssh/agent).

2. Edit `.ssh/config` to have an entry as follows:

    ```bash
    Host vm
        User username_goes_here
        HostName ip_goes_here
        IdentityFile ~/.ssh/id_ed25519
        LocalForward 9999 localhost:8888
    ```

3. Use `ssh-copy-id vm` to copy your ssh key to the server.

4. Start a webserver in your VM by executing `python -m http.server 8888`. Access the VM webserver by navigating to `http://localhost:9999` in your machine.

5. Edit your SSH server config by doing `sudo vim /etc/ssh/sshd_config` and disable password authentication by editing the value of `PasswordAuthentication`. Disable root login by editing the value of `PermitRootLogin`. Restart the `ssh` service with `sudo service sshd restart`. Try sshing in again.

6. (Challenge) Install [`mosh`](https://mosh.org/) in the VM and establish a connection. Then disconnect the network adapter of the server/VM. Can mosh properly recover from it?

7. (Challenge) Look into what the `-N` and `-f` flags do in `ssh` and figure out a command to achieve background port forwarding.