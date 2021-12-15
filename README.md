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
> git clone https://github.com/NVIDIA/nvidia-persistenced.git

> cd nvidia-persistenced/init

> ./install.sh

Verify that the service is running using:
>systemctl status nvidia-persistenced

And check if the nvidia files were created with:
>ls -l /dev/nv*

# Increase Swap file size (Manjaro 512MB)
Disable all swap's
> sudo swapoff -a

Now here's the thing, we can increase the size of the default *swapfile* located in **/swap/swapfile** or we can create a new swap file in another drive.
To do this go the place where you want the swap file and run the commands below:
>sudo dd if=/dev/zero of=swapfile bs=1G count=8 

Where **count** = the size we want in GB 
>sudo chmod 600 swapfile

>sudo mkswap swapfile

>sudo swapon swapfile

Verify the swap was created with htop or other program.

To make this swap permanent run this command:
> sudo nano /etc/fstab

In the **fstab** we comment the other swap entries and paste the code below:
>/PATH/TO/SWAP/FILE/swapfile none swap sw 0 0

Reboot and the swap changes will take effect.


