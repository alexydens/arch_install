# Arch Installation
## Main Installation
1. Load keyboard layout: `loadkeys uk`.
2. Configure wifi:
    - `iwctl`:
        - `device list`.
        - `adapter <ADAPTER> set-property Powered on`.
        - `device <DEVICE> set-property Powered on`.
        - `station <DEVICE> scan`.
        - `station <DEVICE> connect <SSID>`.
3. Test network connection: `ping archlinux.org`.
4. Partitioning the disk:
    - `cfdisk`:
        - EFI Partition: `New`, `100M`..
        - Swap Partition: `New`, `4G`..
        - Root partition: `New`, (use defualt to fill disk).
5. Formatting partitions:
    - `mkfs.ext4 <EFI Partition>`.
    - `mkfs.fat -F32 <Root Partition>`.
    - `mkswap <Swap Partition>`.
6. Mounting partitions:
    - `mount <Root Partition> /mnt`.
    - `mkdir -p /mnt/boot/efi`.
    - `mount <EFI Partition> /mnt/boot/efi`.
    - `mkswap <Swap Partition>`.
7. Check partitions:
    - `lsblk`:
        - 1 line: `100M`, `/mnt/boot/efi`.
        - 1 line: `4G`, `[SWAP]`.
        - 1 line: (very large), `/mnt`.
8. Get base packages: `pacstrap /mnt base linux linux-firmware sof-firmware
base-devel grub efibootmgr vim networkmanager intel-ucode man-db man-pages
texinfo iw git`
9. Generate file system table: `genfstab /mnt > /mnt/etc/fstab`.
10. Change root into new system: `arch-chroot /mnt`.
11. Set time zone: `ln -sf /usr/share/zoneinfo/Europe/London /etc/localtime`
12. Synchronize system clock: `hwclock --systohc`.
13. Generate locale:
    - `vim /etc/locale.gen`, find `en_GB.UTF-8 UTF-8`, uncomment, save.
    - `locale-gen`.
14. Configure locale:
    - `vim /etc/locale.conf`, add `LANG=en_GB.UTF-8`.
15. Set keyboard layout:
    - `vim /etc/vconsole.conf`, add `KEYMAP=uk`.
16. Set hostname:
    - `vim /etc/hostname`, add `lenovo-arch`.
17. Set password: `passwd`.
18. Add user: `useradd -m -G wheel -s /bin/bash <USERNAME>`.
19. Set user password: `passwd <USERNAME>`.
20. Edit file for root privileges:
    - `visudo`, uncomment line beginning `%wheel`.
21. Enable network manager: `systemctl enable NetworkManager`.
22. Install grub: `grub-install <PATH-TO-DRIVE>`.
    - Examples of `<PATH-TO-DRIVE>` include `/dev/sda` and `/dev/nvme0n1`.
23. Make grub configuration: `grub-mkconfig -o /boot/grub/grub.cfg`.
24. Exit installation environment: `exit`, `umount -a`, `reboot`.
## Install yay for AUR Packages
1. Clone yay repository: `git clone https://aur.archlinux.org/yay.git`.
2. Build yay: `cd yay && makepkg -si`.
3. Test: `yay -Syu`.
## Packages
- `tree`.
- `neofetch`.
- `cmatrix`.
- `cowsay`.
- `fzf`.
- `ripgrep`.
- `nodejs`.
- `npm`.
- `curl`.
- `openssh`.
- `net-tools`.
- `ca-certificates`.
- `cmake`.
- `clang`.
- `ccls`.
- `gdb`.
- `valgrind`.
- Rust:
    - Check website.
    - `mkdir -p ~/.config/rustfmt`.
    - `touch ~/.config/rustfmt/rustfmt.toml`.
    - `echo -e "\ntab_spaces = 2" >> ~/.config/rustfmt/rustfmt.toml`.
- `python-pip`.
- `python-pynvim`.
- `flex`.
- `bison`.
- `qemu-base`.
- `nasm`.
- `mtools`.
- `asm_lsp`: `cargo install asm-lsp`.
- `neovim`.
- `npm install -g neovim`.
- `docker`, `sudo systemctl enable docker`.
- `blueman`.
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
