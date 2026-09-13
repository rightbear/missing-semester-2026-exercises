# Exercises

> Testing Environment: 
> 1. Windows11 + WSL2 (Ubuntu 24.04 LTS)

## Practices

1. YEnable Vim mode in all the software you use that supports it, such as your editor and your shell, and use Vim mode for all your text editing for the next month. Whenever something seems inefficient, or when you think "there must be a better way", try Googling it, there probably is a better way.

    ## **Answer**
    Install [the Vim plugin for VS Code](https://marketplace.visualstudio.com/items?itemName=vscodevim.vim).

2. Complete a challenge from [VimGolf](https://www.vimgolf.com/).

    ## **Answer**
    Before starting to play VimGolf, register an account on the VimGolf website and get your own VimGolf key first. My VimGolf key is `5c3a1326bdc9a54457a8f3415c4835b3`.

    ### Demo (Game example: [Simple text editing with Vim](https://www.vimgolf.com/challenges/4d1a34ccfa85f32065000004))
    ```console
    rightbear@Rightbear:~ $ sudo gem install vimgolf
    Fetching vimgolf-0.5.0.gem
    Fetching thor-1.5.0.gem
    Fetching json_pure-2.8.1.gem
    Fetching highline-2.1.0.gem
    Successfully installed thor-1.5.0
    Successfully installed json_pure-2.8.1
    Successfully installed highline-2.1.0

    ------------------------------------------------------------------------------
    Thank you for installing vimgolf-0.5.0.

    0.1.3: custom vimgolf .vimrc file to help level the playing field
    0.2.0: proxy support, custom diffs + proper vimscript parser/scoring
    0.3.0: improve windows support, switch to YAML to remove c-ext dependency
    0.4.0: improved diff/retry CLI, emacs support: http://bit.ly/yHgOPF

    *NOTE*: please re-run "vimgolf setup" prior to playing!

    For more information, rules & updates: http://vimgolf.com/about
    ------------------------------------------------------------------------------
    Successfully installed vimgolf-0.5.0
    Parsing documentation for thor-1.5.0
    Installing ri documentation for thor-1.5.0
    Parsing documentation for json_pure-2.8.1
    Installing ri documentation for json_pure-2.8.1
    Parsing documentation for highline-2.1.0
    Installing ri documentation for highline-2.1.0
    Parsing documentation for vimgolf-0.5.0
    Installing ri documentation for vimgolf-0.5.0
    Done installing documentation for thor, json_pure, highline, vimgolf after 1 seconds
    4 gems installed
    rightbear@Rightbear:~ $ vimgolf setup

    Let's setup your VimGolf key...
    1) Open vimgolf.com in your browser.
    2) Click "Sign in with Twitter".
    3) Once signed in, copy your key (black box, top right).
    Paste your VimGolf key: 5c3a1326bdc9a54457a8f3415c4835b3
    Saved. Happy golfing!
    rightbear@Rightbear:~ $ vimgolf put 4d1a34ccfa85f32065000004
    Downloading Vimgolf challenge: 4d1a34ccfa85f32065000004
    Launching VimGolf session for challenge: 4d1a34ccfa85f32065000004

    Here are your keystrokes:
    :g/V/t.|+d<CR>ZZ

    Success! Your output matches. Your score: 13
    [w] Upload result and retry the challenge
    [x] Upload result and quit
    [r] Do not upload result and retry the challenge
    [q] Do not upload result and quit
    Choice> x
    Uploading to VimGolf...
    Uploaded entry.
    View the leaderboard: https://www.vimgolf.com/challenges/4d1a34ccfa85f32065000004

    Thanks for playing!
    ```

    ### Explanation

    Type the following sequence `g/V/t.|+d` in Normal mode and press <Enter> to execute then press `ZZ` to store the results:`:g/V/t.|+d<Enter>ZZ`.
    This solution leverages Vim's powerful Ex commands to handle the task efficiently:
    `:g/V/`: Globally search for every line containing the uppercase letter `V` (the starting character of the source lines).
    `t.`: Copy (`t`) the matched line to the current cursor position (`.`), which inserts it right above the next line.
    `d`: Delete that line, effectively overwriting it with the newly copied text.
    `<Enter>`: Run the chained Ex commands for the whole file.
    `ZZ`: Save the changes and exit.
    By using the global command `:g`, you eliminate the need for repetitive manual cursor movements or macro recordings

3. Configure an IDE extension and language server for a project that you're working on. Ensure that all the expected functionality, such as jump-to-definition for library dependencies, works as expected. If you don't have code that you can use for this exercise, you can use some open-source project from GitHub (such as [this one](https://github.com/spf13/cobra)).

    ## **Answer**
    If you are developing in Python using VS Code, you can install [the Python extension for VS Code](https://marketplace.visualstudio.com/items?itemName=ms-python.python) as the language server provided by the extension. To verify that the language server is working correctly, you can set up a Python virtual environment(like `venv`), install a third-party library, and test features like 'Go to Definition' on the library's dependencies.

4. Browse a list of IDE extensions and install one that seems useful to you.

    ## **Answer**
    Install [Remote - SSH](https://marketplace.visualstudio.com/items?itemName=ms-vscode-remote.remote-ssh).