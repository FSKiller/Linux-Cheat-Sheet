# Linux Journey Cheat Sheet
## FIX NVIDIA DX12 GAMES (In Lutris)
To enable DX12 compatibilty in linux with NVIDIA GPU's run:
- sudo nvidia-modprobe -u -c=0 (This setting resets after after reboot.)

To enable DX12 compatibilty permanently run:
- nano /etc/modules-load.d/modules.conf // Add nvidia e nvidia_uvm to this file (One per line).
- nano /etc/udev/rules.d/70-nvidia.rules // Inside this file add: 
	> #Create /nvidia0, /dev/nvidia1 … and /nvidiactl when nvidia module is loaded
	> KERNEL=="nvidia", RUN+="/bin/bash -c '/usr/bin/nvidia-smi -L'"
	> #Create the CUDA node when nvidia_uvm CUDA module is loaded
	> KERNEL=="nvidia_uvm", RUN+="/bin/bash -c '/usr/bin/nvidia-modprobe -c0 -u'"

Go to a new directory and the follow the commands below:
>git clone https://github.com/NVIDIA/nvidia-persistenced.git
cd nvidia-persistenced/init
./install.sh

Verify that the service is running using:
>systemctl status nvidia-persistenced

And check if the nvidia files were created with:
>ls -l /dev/nv*

# Increase Swap file size (Manjaro default is 512MB)
Disable all swap's
> sudo swapoff -a

Now here's the thing we can increase the size of the default *swapfile* locates in **/swap/swapfile** or we can create a new swap file in another drive.
To do this go the place where you want the swap file and run the commands below:
>sudo dd if=/dev/zero of=swapfile bs=1G count=8 //Where **count** = the size we want in GB 
sudo chmod 600 swapfile
sudo mkswap swapfile
sudo swapon swapfile

Verify the swap was created with htop or other program.

To make this swap permanent run this command:
> sudo nano /etc/fstab

In the **fstab** we comment the other swap entries and paste the code below:
>/PATH/TO/SWAP/FILE/swapfile none swap sw 0 0

Reboot and the swap changes will take effect.

Check Game mode is running:
gamemoded -s


# Enable Hardware Accel in Chrome:
Create a file in:
> ~/.config/chrome-flags.conf

With these flags:

	--ignore-gpu-blocklist
	--enable-gpu-rasterization
	--enable-zero-copy
	--enable-features=VaapiVideoDecoder
	--use-gl=desktop
	--canvas-oop-rasterization

# Installing linux-tkg Kernel
Git Clone from tkg's repo (Arch and derivs Only):
> https://github.com/Frogging-Family/linux-tkg
> Edit "customization.cfg" for specific patches
> makepkg -si

# Getting "EMP.dll Games" running
Download a Non MinGW wine Version.

# Running WineTricks manually:
> export WINEPREFIX=/home/$User/Games/$Game/Prefix/

> env WINE=/home/$User/.local/share/lutris/runners/wine/lutris-ge-7.9-2-x86_64/bin/wine64 winetricks

# Using Online Fix with Steam
> WINEDLLOVERRIDES="OnlineFix64=n;winmm=n,b;elden_ring_seamless_coop=n;steam_api64=n" OBS_VKCAPTURE=1 mangohud %command%

>WINEDLLOVERRIDES="steam_api64_o=n;steam_api64=n" __GL_SHADER_DISK_CACHE_SKIP_CLEANUP=1 __GL_THREADED_OPTIMIZATIONS=1 PROTON_ENABLE_NGX_UPDATER=1 DXVK_NVAPI_DRIVER_VERSION=52849 PROTON_ENABLE_NVAPI=1 VKD3D_CONFIG=dxr11 VKD3D_FEATURE_LEVEL=12_1 OBS_VKCAPTURE=1 mangohud protonhax init %command%

# Compiling Dxvk and VKD3D(for reference)
> ./package-release.sh master ./Builds/ --no-package

# Fixing possible network errors in Ubisoft Connect
You can enable it temporarily with `sudo sysctl -w net.ipv4.tcp_mtu_probing=1`

And to make it permanent, run `echo "net.ipv4.tcp_mtu_probing=1" | sudo tee /etc/sysctl.d/90-custom-mtu-probing.conf`

> tcp_mtu_probing 
>
> Controls TCP Packetization-Layer Path MTU Discovery. Takes three values:
>
> 0 - Disabled
>
> 1 - Disabled by default, enabled when an ICMP black hole detected
>
> 2 - Always enabled, use initial MSS of tcp_base_mss.

# Using Dracut and EOS with Nvidia Part 1
Install this:
> https://github.com/endeavouros-team/PKGBUILDS/tree/master/nvidia-hook
Then create a nvidia hook for pacman.
