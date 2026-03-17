# Arch Base System

Base system for Arch Linux installer (Calamares).

## Download

```bash
wget https://github.com/lucasgertke11-bot/arch-base/releases/latest/download/arch-base.tar.xz
```

## Extract for Installation

```bash
# Extract to target mount point (e.g., /mnt)
tar -xf arch-base.tar.xz -C /mnt

# Or extract to current directory
tar -xf arch-base.tar.xz
```

## Calamares Integration

In your Calamares configuration, use a post-install script or module to:

1. Download and extract the base system during installation
2. Configure the system (hostname, locale, etc.)
3. Install additional packages as needed

Example post-install script:

```bash
#!/bin/bash
set -e

# Download base system
wget -q https://github.com/lucasgertke11-bot/arch-base/releases/latest/download/arch-base.tar.xz

# Extract to target
tar -xf arch-base.tar.xz -C /mnt

# Clean up
rm -f arch-base.tar.xz

# Generate fstab
genfstab -U /mnt >> /mnt/etc/fstab

# Chroot and configure
arch-chroot /mnt /bin/bash -c "
    # Set timezone
    ln -sf /usr/share/zoneinfo/UTC /etc/localtime
    hwclock --systohc

    # Set locale
    echo 'en_US.UTF-8 UTF-8' > /etc/locale.gen
    locale-gen
    echo 'LANG=en_US.UTF-8' > /etc/locale.conf

    # Set hostname
    echo 'archlinux' > /etc/hostname

    # Initramfs
    mkinitcpio -P

    # Set root password
    passwd
"

echo "Installation complete!"
```

## System Requirements

- x86_64 architecture
- Minimum 512MB RAM
- 500MB disk space (base only)

## What's Included

- Core Arch Linux base system
- Systemd
- Basic libraries and tools
- Kernel (need to install kernel separately or use host kernel)

## Update Base System

To create a new base system:

```bash
sudo pacstrap -i /mnt base base-devel
sudo rm -rf /mnt/var/cache/pacman/*
sudo tar -cvJf arch-base.tar.xz -C /mnt .
```
