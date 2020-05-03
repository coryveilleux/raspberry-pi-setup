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

## Initial Raspberry Pi Configuration

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

    i. Change your password - option 1

    ii. Configure WiFi - option 2 (Network Options), then option N2 (Wi-fi)

        a.  Set your country

        b.  Enter your SSID

        c.  Enter your passphrase

    iii. Enable VNC - option 5 (Interfacing Options), then option P3 (VNC)

    iv. Fix resolution - option 7 (Advanced Options), then A5 (Resolution)

        a. Choose the last option `DMT Mode 82 1920x1080 60Hz 16:9`

        b. Choose to reboot now

3.  Perform initial update:

    ```bash
    sudo apt update
    sudo apt full-upgrade
    sudo apt clean
    sudo reboot
    ```

4.  Install development-related things:

    i. gVim:

        ```bash
        sudo apt install vim-gui-common vim-gtk
        ```

    ii. zsh:

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

    iii. Oh-my-zsh:

        ```bash
        sh -c "$(curl -fsSL https://raw.github.com/ohmyzsh/ohmyzsh/master/tools/install.sh)"
        ```

    iv. powerlevel10k:

        ```bash
        git clone https://github.com/ryanoasis/nerd-fonts.git
        cd nerd-fonts
        ./install.sh Meslo
        git clone --depth=1 https://github.com/romkatv/powerlevel10k.git ${ZSH_CUSTOM:-~/.oh-my-zsh/custom}/themes/powerlevel10k
        sed -i.bak 's#ZSH_THEME="[^"]*"#ZSH_THEME="powerlevel10k/powerlevel10k"#' ~/.zshrc
        zsh
        # or later...
        p10k configure
        # TODO
        ```

    v. Visual Studio Code:

        ```bash
        sudo -s
        . <( wget -O - https://code.headmelted.com/installers/apt.sh )
        ```

TODO

    vi. pyenv:

        ```bash
        git clone https://github.com/pyenv/pyenv.git ~/.pyenv
        echo 'export PYENV_ROOT="$HOME/.pyenv"' >> ~/.zshrc
        echo 'export PATH="$PYENV_ROOT/bin:$PATH"' >> ~/.zshrc
        echo -e 'if command -v pyenv 1>/dev/null 2>&1; then\n  eval "$(pyenv init -)"\nfi' >> ~/.zshrc
        ```
