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
> export WINEPREFIX=/home/thunderfox/Games/Elden\ Ring/Prefix/

> env WINE=/home/thunderfox/.local/share/lutris/runners/wine/lutris-ge-7.9-2-x86_64/bin/wine64 winetricks
