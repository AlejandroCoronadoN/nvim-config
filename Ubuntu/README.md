#Install Dependencies - Ubutnu 20
## Setup Oh My ZSH

```
sudo apt-get update -y && sudo apt-get upgrade -y
sudo apt-get install zsh -y
sudo apt-get install git-core -y
sh -c "$(curl -fsSL https://raw.github.com/robbyrussell/oh-my-zsh/master/tools/install.sh)"

sudo chsh -s $(which zsh) $(whoami)
```

Install conda and libraries. First Download anaconda directly from this link and save it at root of the Ubuntu instance. In WSL (Windows) it will be in a path that look like this "\\ws    l.localhost\Ubuntu-20.04\home\wallfacer" :

https://docs.conda.io/en/latest/miniconda.html#linux-installers
```
bash Anaconda-latest-Linux-x86_64.sh
```
This install is going to install the latest version of conda and add the following code to the .zshrc
```

# >>> conda initialize >>>
# !! Contents within this block are managed by 'conda init' !!
__conda_setup="$('/home/wallfacer/anaconda3/bin/conda' 'shell.zsh' 'hook' 2> /dev/null)"
if [ $? -eq 0 ]; then
    eval "$__conda_setup"
else
    if [ -f "/home/wallfacer/anaconda3/etc/profile.d/conda.sh" ]; then
        . "/home/wallfacer/anaconda3/etc/profile.d/conda.sh"
    else
        export PATH="/home/wallfacer/anaconda3/bin:$PATH"
    fi
fi
unset __conda_setup
# <<< conda initialize <<<  
```


Install important plugins. You need to install conda and pip to use some of these libraries.

```

git clone https://github.com/MichaelAquilina/zsh-you-should-use.git $ZSH_CUSTOM/plugins/you-should-use
git clone https://github.com/zsh-users/zsh-history-substring-search ${ZSH_CUSTOM:-~/.oh-my-zsh/custom}/plugins/zsh-history-substring-searc

 git clone https://github.com/zsh-users/zsh-autosuggestions ${ZSH_CUSTOM:-~/.oh-my-zsh/custom}/plugins/zsh-autosuggestions
```
## Neovim
Install complementary libraries including, zip, npm.

```
sudo apt install zip 
sudo dnf install unzip
sudo apt-get install wget

wget -qO- https://raw.githubusercontent.com/creationix/nvm/v0.39.0/install.sh | bash
source ~/.zshrc
nvm install 18 
```

Replace the .zshrc file by the one provided in this directory.  After that we need to Download and install the latest neovim version from this repository. Download and save into the root directory and extract the content. Reference: https://www.youtube.com/watch?v=2wapxsfzLho&t=145s
 
https://github.com/neovim/neovim/releases 

Once dowloaded extract with 

```
tar xzvf nvim-linux64.tar.gz

```
### Sudo permission

Copy the extracted file to .local/bin  and create a symbolic link that allows us to execute nvim from sudo and access local setup.

```
mv -r nvim-linux64 /home/wallfacer/.local/bin/nvim-linux64 
ln -s ./nvim-linux64/bin/nvim ./nvim 
```

This will allow us to executo sudo vim, however sudo vim will not use the lua environement. To achieve this chage the sudders file
```
sudo visudo 

```
Change the default editor line and choose nvim and comment the line Defaults sequre_path="/usr/local/sbin:usr/local/bin : ...". This action will allow to execute any program using sudo, not only nvim. Only apply this change if you want to use sudo nvim to edit configuration files.
```
Defaults editor=/home/wallfacer/.local/bin/nvim
#Defaults   sequre_path="..."

```
### Mason
For the mason configiration is better to select the e LSP servers, DAP servers, linters, and formatters individually. To do this start nvim and use the COMMAND mode and type :Mason 
This will open a list of all the available linters and servers. Choose all the ones you need by moving to the line and pressing i for install.   It will start installing in the background.

Remember to install zip before running this command. If the install fails, use :MasonLogs to read errors. If you ecounter the "rmrf was cannot be used" error, then you will need to remove the file manually since nvim can't delete files without the proper permissions.

### Fonts and Icons - WSL (Windows Ubuntu). 
First make sure you are using windows Terminal. Windows terminal can be installed from the Microsoft Store. 

In order for the  oh-my-zsh agnoster theme to display icons and labels we need to install a modern font. You can use the Hack font from Nerdfonts.com and set it as default. To do this just open windows terminal and click on the + symbol that appears next to the new terminal button (top right corner). Under apperaces and option there is a list of all the OS system linked with Windows terminal. The First option (Defaults) apply the same changes to all the terminal. Choose a font and a theme. You can import a the by comping and pasting a json file into the settings Json object. To open the windows terminal settings json, click on the bottom lft corner and select Open JSON File and add the follwing Gruvbox configuration

```
        {
            "background": "#010F10",
            "black": "#282828",
            "blue": "#1C4F4F",
            "brightBlack": "#928374",
            "brightBlue": "#83A598",
            "brightCyan": "#8EC07C",
            "brightGreen": "#B8BB26",
            "brightPurple": "#D3869B",
            "brightRed": "#FB4934",
            "brightWhite": "#EBDBB2",
            "brightYellow": "#FABD2F",
            "cursorColor": "#FFFFFF",
            "cyan": "#689D6A",
            "foreground": "#EBDBB2",
            "green": "#98971A",
            "name": "Gruvbox Dark",
            "purple": "#B16286",
            "red": "#CC241D",
            "selectionBackground": "#FFFFFF",
            "white": "#A89984",
            "yellow": "#D79921"
        },
```

### Test Commands 
Inmediately nvim will change entirely by installing and applying all out configuration. The most important features to test are

* **space + sv:** split screen vertically
* **space + sh:** split screen horizontally
* **space + sx:** Close screen
* **space + se:** resize windows to same size 

* **space + to:** New tab
* **space + tx:** Close tab
* **space + tn:** Next Tab
* **space + tp:** Previous Tab
* 
#### LSP
* ** [d :** Go to the next lsp diagnostic
* ** Q:** Quit diagnostic
* **gd:** split screen vertically
* **gR:** Show LSP references"
* **gD:** Go to declaration"
* **gd:** Show LSP definitions"
* **gi:** Show LSP implementations"
* **gt:** Show LSP type definitions"
* **space + ca:** See available code actions"
* **space +rn:** Smart rename"
* **space + D:** Show buffer diagnostics"
* **space + d:** Show line diagnostics"
* ** [d:** Go to previous diagnostic"
* ** ]d:** Go to next diagnostic"
* ** K:** Show documentation for what is under cursor"
* ** space + rs:** Restart LSP"
  

#### Telescope
Before start using telescope, be sure to install this library, it will allow live_grep to work and search files by sub-string matching.
```
sudo apt-get install ripgrep
```

* ** space +ff: **Telescope find_files - Fuzzy find files in cwd
* ** space +fr: **Telescope oldfiles - Fuzzy find recent files
* ** space +fs: **Telescope live_grep - Find string in cwd
* ** space +fc: **Telescope grep_string - Find string under cursor in cwd

#### Tree NVIM
* ** space + ee: **Toggle file explorer
* ** space + ef: **"Toggle file explorer on current fileexplorer on current file
* ** space + ec: **"Collapse file explorer
* ** space + er: **Refresh file explorer

###  Tmux
Install inside WSL (Ubuntu) and install tmux package manager
```
sudo apt install tmux
git clone https://github.com/tmux-plugins/tpm ~/.tmux/plugins/tpm
```
copy the tmux.config file to use shortcuts and tmux features. Use the follwing commands to Create, detach and reatach tmux sessions.
```
tmux new -s SESION_NAME
tmux detach
tmux attach -t SESSION_NAME
```~~~~

#### Tmux Shortcuts
Ctrl + A is the tmux prefix that allows us to use tmux commands directly after using Ctrl+a. 

* ** ctrl + a + s: ** List all tmux sessions 
* ** ctrl + a + -: ** Split Horizontally and create terminals
* ** ctrl + a + |: ** Split Vertically and create terminals
* ** ctrl + a + h,l,j,k: ** Resize Horizontally or Vertically in the corresponding direction. 
* ** ctrl + a + I: ** To realod and install plugins
* 


## Debuuggers
### Python Debuuggers

Install debugpy in a conda environment. Is important that the python version matches the python installed in the system and to be as recent as possible. The debugging.lua file exepects that envrionment trough the CONDA_PREFIX. 

```
conda cretae --name debugpy  -c conda-forge python=3.11
conda activate debugpy 
conda install conda-forge::debugpy
```
You will need to adjust the python debugger setup in debugging.lua and chage it for your own path
/home/wallfacer/anaconda3/envs/debugpy/bin/python


lua require('dap-python').setup('~wallfacer/anaconda3/envs/debugpy/bin/python')