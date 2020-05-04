# Raspberry Pi Setup

## Download and Etch to Micro SD Card

1.  Download the latest _Raspbian with desktop_ from
    https://www.raspberrypi.org/downloads/raspbian/:
    https://downloads.raspberrypi.org/raspbian_latest

2.  Download [Etcher](https://www.balena.io/etcher/)

3.  Etch Raspbian zip to your micro SD card with Etcher.
    In settings, uncheck _Auto-unmount on success_.

    ![](./images/etcher-settings.png)

4.  Enable ssh for headless setup:

    ```bash
    cd /Volumes/boot
    touch ssh
    ```

## Start Up Raspberry Pi

1.  Plug raspberry pi into an ethernet port on your router and connect it to
    power.

2.  Use your router web interface or app to determine what IP address has been
    given to your raspberry pi.

## Initial Raspberry Pi Configuration and Update

1.  SSH into your raspberry pi (default user is `pi` and default password is
    `raspberry`):

    ```bash
    ssh pi@[raspberry pi IP address]
    # enter raspberry as the password
    ```

2.  Perform initial configuration for your raspberry pi:

    ```bash
    sudo raspi-config
    ```

    ![](./images/raspi-config-home.png)

    Use the arrows keys to move between option, the TAB key to switch between
    buttons, and the ENTER key to make a selection.

    1. Change your password - option 1

    2. Configure WiFi - option 2 (Network Options), then option N2 (Wi-fi)

        1.  Set your country

        2.  Enter your SSID

        3.  Enter your passphrase

    3. Enable VNC - option 5 (Interfacing Options), then option P3 (VNC)

    4. Fix resolution - option 7 (Advanced Options), then A5 (Resolution)

        1. Choose the last option `DMT Mode 82 1920x1080 60Hz 16:9`

        2. Choose to reboot now

3.  Perform initial update:

    ```bash
    sudo apt update
    sudo apt full-upgrade
    sudo apt clean
    sudo reboot
    ```

## Software Installation

### Shells / Prompts

#### zsh

```bash
sudo apt install zsh
chsh -s /bin/zsh
# exit and ssh back in
# Choose 1 to setup zsh config
# Leave History (1) as is
# Configure Completion (2) - select 1 to turn on default completion
# Choose 3 to configure editing - select 1 and then v
# Choose 4 to configure shell options - 1s, 2s
# Choose 0 to save and exit
```

#### [Oh My Zsh](https://ohmyz.sh/)

```bash
sh -c "$(curl -fsSL https://raw.github.com/ohmyzsh/ohmyzsh/master/tools/install.sh)"
```

##### Plugins

###### Standard

```bash
sed -i.$(date +'%Y%m%d-%H%M%S') 's/^plugins=([^)]*)$/plugins=(git python pip pipenv pyenv docker vscode virtualenv)/' ~/.zshrc
```

###### zsh-autosuggestions & zsh-syntax-highlighting

```bash
git clone https://github.com/zsh-users/zsh-autosuggestions.git $ZSH_CUSTOM/plugins/zsh-autosuggestions
git clone https://github.com/zsh-users/zsh-syntax-highlighting.git $ZSH_CUSTOM/plugins/zsh-syntax-highlighting
sed -i.$(date +'%Y%m%d-%H%M%S') 's/^\(plugins=([^)]*\))$/\1 zsh-autosuggestions zsh-syntax-highlighting)/' ~/.zshrc
```

###### autojump

```bash
git clone git://github.com/wting/autojump.git
cd autojump
./install.py
echo '\n# autojump' >> ~/.zshrc
echo "[[ -s $HOME/.autojump/etc/profile.d/autojump.sh ]] && source $HOME/.autojump/etc/profile.d/autojump.sh" >> ~/.zshrc
echo 'autoload -U compinit && compinit -u' >> ~/.zshrc
# restart terminal
```

###### zsh-auto-venv

This will automatically load a python3
[venv module](https://docs.python.org/3/library/venv.html) virtual environment 
if a `venv` directory is present in the current directory hierarchy:

```bash
git clone https://github.com/ikhomutov/zsh-auto-venv.git $ZSH_CUSTOM/plugins/zsh-auto-venv
sed -i.$(date +'%Y%m%d-%H%M%S') 's/^\(plugins=([^)]*\))$/\1 zsh-auto-venv)/' ~/.zshrc
```

#### [powerlevel10k](https://github.com/romkatv/powerlevel10k)

First you need to install nerd fonts:

##### Nerd Fonts

1. On raspberry pi via the terminal/ssh:

    ```bash
    # Note that this clone takes a long time (~11GB)
    git clone https://github.com/ryanoasis/nerd-fonts.git
    cd nerd-fonts
    ./install.sh Meslo
    ```

2. On raspberry pi via the desktop/VNC:

    1. Open the terminal app
   
    2. Go to _Edit_ > _Preferences_
   
    3. On the _Style_ tab choose `MesloLGS Nerd Font Mono Regular`

        * NOTE: Here's a discussion of the `LG`, `L`, `M`, `S`, `DZ`, and `SZ`
          acronyms:
          https://github.com/ryanoasis/nerd-fonts/wiki/FAQ-and-Troubleshooting#what-do-these-acronym-variations-in-the-font-name-mean-lg-l-m-s-dz-sz

    4. Also on the _Style_ tab, change the _Palette_ to `Solarized Dark`, and
       then change again to `Custom`. Set the bottom left color to the custom
       `#586D74` (this is the same color as the fixed Solarized Dark theme for
       Mac below).

3. On Mac (use [iTerm2](https://iterm2.com/)):

    ```bash
    brew tap homebrew/cask-fonts
    brew cask install font-meslolg-nerd-font
    ```

    Also you might want to use this fixed Solarized Dark theme from
    https://gist.github.com/kevin-smets/8568070:

    1. Download https://raw.githubusercontent.com/mbadolato/iTerm2-Color-Schemes/master/schemes/Solarized%20Dark%20-%20Patched.itermcolors

    2. In iTerm2 preferences, go to _Preferences_ > _Profiles_ > _Default_ >
       _Colors_ > _Color Presets..._ and import 
       `Solarized Dark - Patched.itermcolors`

Now continue with the powerlevel10k install:

```bash
git clone --depth=1 https://github.com/romkatv/powerlevel10k.git ${ZSH_CUSTOM:-~/.oh-my-zsh/custom}/themes/powerlevel10k
sed -i.$(date +'%Y%m%d-%H%M%S') 's#ZSH_THEME="[^"]*"#ZSH_THEME="powerlevel10k/powerlevel10k"#' ~/.zshrc
zsh
# or later...
p10k configure
# Answer initial questions - should be 4 y, or 3 y and then n
# My current prefs...
# Prompt Style - 3 (Rainbow)
# Character Set - 1 (Unicode)
# Show current time? - 3 (12-hour format)
# Prompt Separators - 4 (Round)
# Prompt Heads - 1 (Sharp)
# Prompt Tails - 1 (Flat)
# Prompt Height - 2 (2 lines)
# Prompt Connection - 2 (Dotted)
# Prompt Frame - 1 (No frame)
# Connection Color - 3 (Dark)
# Prompt Spacing - 2 (Sparse)
# Icons - 2 (Many icons)
# Prompt Flow - 2 (Fluent)
# Enable Transient Prompt? - n (No)
# Instant Prompt Mode - 3 (Verbose)
# Overwrite ~/.p10k.zsh? - y (Yes)
```

### Editors/IDEs

#### gVim

```bash
sudo apt install vim-gui-common vim-gtk
```

#### [Visual Studio Code](https://code.headmelted.com/)

```bash
sudo -s
. <( wget -O - https://code.headmelted.com/installers/apt.sh )
```

### Language Tools/Frameworks

#### [pyenv](https://github.com/pyenv/pyenv)

```bash
git clone https://github.com/pyenv/pyenv.git ~/.pyenv
echo '\n# pyenv' >> ~/.zshrc
echo 'export PYENV_ROOT="$HOME/.pyenv"' >> ~/.zshrc
echo 'export PATH="$PYENV_ROOT/bin:$PATH"' >> ~/.zshrc
echo -e 'if command -v pyenv 1>/dev/null 2>&1; then\n  eval "$(pyenv init -)"\nfi' >> ~/.zshrc
```

#### pynv Python Install

```bash
sudo apt install python-openssl libssl-dev libbz2-dev libedit-dev libreadline-dev libsqlite3-dev
pyenv install 3.8.2
pyenv install 3.6.10
pyenv install 3.5.9
```

#### java

```bash
# java 11
sudo apt install default-jdk
# java 8
sudo apt install openjdk-8-jdk
```

#### jenv

```bash
git clone https://github.com/jenv/jenv.git ~/.jenv

echo '\n# jenv' >> ~/.zshrc
echo 'export PATH="$HOME/.jenv/bin:$PATH"' >> ~/.zshrc
echo 'eval "$(jenv init -)"' >> ~/.zshrc

jenv add /usr/lib/jvm/java-11-openjdk-armhf/
jenv add /usr/lib/jvm/java-8-openjdk-armhf/
jenv enable-plugin export

sed -i.$(date +'%Y%m%d-%H%M%S') 's/^\(plugins=([^)]*\))$/\1 jenv)/' ~/.zshrc
```

#### scala

```bash
wget https://downloads.typesafe.com/scala/2.13.2/scala-2.13.2.tgz
sudo mkdir /usr/lib/scala
sudo tar -xf scala-2.13.2.tgz -C /usr/lib/scala
rm scala-2.13.2.tgz
sudo ln -s /usr/lib/scala/scala-2.13.2/bin/scala /bin/scala
sudo ln -s /usr/lib/scala/scala-2.13.2/bin/scalac /bin/scalac
scala --version
```
