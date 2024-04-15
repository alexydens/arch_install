# Arch Installation
## Main Installation
1. Load keyboard layout: `loadkeys uk`.
2. Test network connection: `ping archlinux.org`.
3. Partitioning the disk:
    - `cfdisk`:
        - EFI Partition: `New`, `100M`..
        - Swap Partition: `New`, `4G`..
        - Root partition: `New`, (use defualt to fill disk).
4. Formatting partitions:
    - `mkfs.ext4 <EFI Partition>`.
    - `mkfs.fat -F32 <Root Partition>`.
    - `mkswap <Swap Partition>`.
5. Mounting partitions:
    - `mount <Root Partition> /mnt`.
    - `mkdir -p /mnt/boot/efi`.
    - `mount <EFI Partition> /mnt/boot/efi`.
    - `mkswap <Swap Partition>`.
6. Check partitions:
    - `lsblk`:
        - 1 line: `100M`, `/mnt/boot/efi`.
        - 1 line: `4G`, `[SWAP]`.
        - 1 line: (very large), `/mnt`.
7. Get base packages: `pacstrap /mnt base linux linux-firmware sof-firmware
base-devel grub efibootmgr vim networkmanager intel-ucode man-db man-pages
texinfo iw git`
8. Generate file system table: `genfstab /mnt > /mnt/etc/fstab`.
9. Change root into new system: `arch-chroot /mnt`.
10. Set time zone: `ln -sf /usr/share/zoneinfo/Europe/London /etc/localtime`
11. Synchronize system clock: `hwclock --systohc`.
12. Generate locale:
    - `vim /etc/locale.gen`, find `en_GB.UTF-8 UTF-8`, uncomment, save.
    - `locale-gen`.
13. Configure locale:
    - `vim /etc/locale.conf`, add `LANG=en_GB.UTF-8`.
14. Set keyboard layout:
    - `vim /etc/vconsole.conf`, add `KEYMAP=uk`.
15. Set hostname:
    - `vim /etc/hostname`, add `lenovo-arch`.
16. Set password: `passwd`.
17. Add user: `useradd -m -G wheel -s /bin/bash <USERNAME>`.
18. Set user password: `passwd <USERNAME>`.
19. Edit file for root privileges:
    - `visudo`, uncomment line beginning `%wheel`.
20. Enable network manager: `systemctl enable NetworkManager`.
21. Connect to WiFi:
    - `nmcli device wifi list`.
    - `nmcli device wifi connect <NETWORK> password <NETWORK-PASSWORD>`
22. Install grub: `grub-install <PATH-TO-DRIVE>`.
    - Examples of `<PATH-TO-DRIVE>` include `/dev/sda` and `/dev/nvme0n1`.
23. Make grub configuration: `grub-mkconfig -o /boot/grub/grub.cfg`.
24. Exit installation environment: `exit`, `umount -a`, `reboot`.
## Install yay for AUR Packages
1. Clone yay repository: `git clone https://aur.archlinux.org/yay.git`.
2. Build yay: `cd yay && makepkg -si`.
3. Test: `yay -Syu`.
## Packages
### Hyprland
- `alacritty` - terminal.
- `dolphin` - file manager.
- `firefox`.
- `hyprland` - display and window manager.
- `hyprlock` - for locking the screen.
- `hyprpaper` - for wallpaper.
- `hyprpicker` - a colour picker.
- `imv` - an image viewer.
- `polkit-kde-agent` - gain privileges.
- `qt5-wayland` - i'm guessing the QT framework?
- `spotify-launcher`.
- `swayidle` - idle management daemon.
- `swaylock-effects` - a nice lock screen.
- `swaync` - notification daemon.
- `waybar` - a bar at the top with info.
- `wlogout` - logout, shutdown, etc. settings.
- `xdg-desktop-portal-hyprland` - windows using xdg.
- `wofi` - menu of things to run.
### Connections
- `blueman`.
- `bluez-utils`.
- `inetutils`.
- `iw`.
- `net-tools`.
- `network-manager`.
### Audio
- `pamixer`.
- `pipewire`.
- `wireplumber`.
- `pipewire-jack`.
- `pipewire-pulse`.
### Graphics
- `lib32-nvidia-utils`.
- `nvidia`.
- `nvidia-settings`.
- `nvidia-dkms`.
- `vulkan-extra-layers`.
- `vulkan-extra-tools`.
- `vulkan-html-docs`.
- `vulkan-tools`.
- `vulkan-utility-libraries`.
### Hardware
- `alsa-firmware`.
- `alsa-oss`.
- `alsa-plugins`.
- `alsa-scarlett-gui`.
- `alsa-tools`.
- `alsa-utils`.
- `brightnessctl`.
- `qpwgraph`.
- `sof-firmware`.
### Programming
- `ccls`.
- `clang`.
- `cmake`.
- `composer` - PHP package manager.
- `docker`.
    - `systemctl --user enable --now docker-desktop`.
- `gdb`.
- `jdk-openjdk`.
- `julia`.
- `luarocks`.
- `nasm`.
- `nodejs`.
- `npm`.
- `python-pip`.
- `python-pynvim`.
- `ruby`.
- `valgrind`.
#### Rust
    - `curl --proto '=https' --tlsv1.2 -sSf https://sh.rustup.rs | sh`
### Base
- `base`.
- `base-devel`.
- `efibootmgr`.
- `git`.
- `grub`.
- `intel-ucode`.
- `linux`.
- `linux-firmware`.
### Fun
- `cmatrix`.
- `cowsay`.
- `fortune-mod`.
- `rig`.
- `sl`.
- `toilet`.
### Manual pages
- `man-db`.
- `man-pages`.
- `texinfo`.
### Utils
- `fzf`.
- `htop`.
- `mtools`.
- `neofetch`.
- `pv`.
- `ripgrep`.
- `tree`.
- `wget`.
- `curl`.
### Editors
- `neovim`.
- `vi`.
- `vim`.
### Fonts
- `ttf-font-awesome`.
- `ttf-jetbrains-mono-nerd`.
### AUR
- `yay`.
- `yay-debug`.
### Other
- `openssh`.
- `qemu-base`.
- `zsh`.
## Install Neovim
1. Install a nerd font: `sudo pacman -S ttf-jetbrains-mono-nerd`.
2. Install neovim: `sudo pacman -S neovim`.
3. Use configuration:
    - `mkdir -p ~/.config/nvim`.
    - `git clone https://github.com/alexydens/nfimcfg.git ~/.config/nvim`.
## Install ZSH
1. Install: `sudo pacman -S zsh`.
2. Run for config:
    - `zsh /usr/share/zsh/functions/Newuser/zsh-newuser-install -f`
    - Follow steps.
3. Install `oh-my-zsh`:
    - Get: `sh -c "$(wget -O- https://raw.githubusercontent.com/ohmyzsh/ohmyzsh/master/tools/install.sh)"`.
    - Get: `git clone https://github.com/zsh-users/zsh-syntax-highlighting.git ${ZSH_CUSTOM:-~/.oh-my-zsh/custom}/plugins/zsh-syntax-highlighting`.
    - Get: `git clone https://github.com/zsh-users/zsh-autosuggestions ${ZSH_CUSTOM:-~/.oh-my-zsh/custom}/plugins/zsh-autosuggestions`.
    - Get: `git clone https://github.com/agkozak/zsh-z $ZSH_CUSTOM/plugins/zsh-z`.
    - Get: `git clone --depth 1 https://github.com/junegunn/fzf.git ~/.fzf`.
    - Run: `~/.fzf/install`.
4. Edit `~/.vshrc`:
    - Add:
    ```
    plugins=(
        fzf
        git
        history-substring-search
        colored-man-pages
        zsh-autosuggestions
        zsh-syntax-highlighting
        zsh-z
    )
    ```
    - Add: `ZSH_THEME="eastwood"`.
5. Set default shell: `chsh -s $(which zsh)`.
## Install Hyprland
- Follow tutorial, then ensure website for nvidia cards.
