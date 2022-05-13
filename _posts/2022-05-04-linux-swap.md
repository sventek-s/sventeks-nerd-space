---
published: true
---

# VMware Workstation & Linux Swap

Linux swap is a space on disk that is used when the amount of physical RAM memory is full.

I had an issue with my vmware where each time I would start a virtual enviroment of 8GB ram while my host system had 32G ram it would give me the below error.
<p align="center">
  <img src="https://user-images.githubusercontent.com/70489395/166643488-af382bc4-810f-4e17-a833-cca3f4c17427.png" alt="image">
</p>

After some research I came to learn that vmware has a setting where the virtual machines can swap virtual memory with host ram, but also you can configure to fit all virtual machine memory into reserved host RAM and that would fix your error too.

But I just want to note on how you can have a swap file which you can turn on and off. The benefit of this setup from my point of view is that it gives you a sense of control.

## Fix 1

> The easy way out.

Start vmware from terminal with sudo, then head to the Edit tab -> Preference -> Memory.

Under the Additional Memory setting change it from `Allow some virtual machine memory to be swapped` to `Fit all virtual machine memory into reserved host RAM`

<p align="center">
  <img src="https://user-images.githubusercontent.com/70489395/166674716-e73570b9-9920-43cf-a9ca-9536fcf918b1.png" alt="image">
</p>

Note that you should use this option if you have enough physical memory on your host machine. Also I tried this option used it for a while and it didn't work well for me as I was expecting but you should give it a try.

## Fix 2

> Abit of work, here and there 

This option allows us to extend swap size to a larger one.

```sh
#first confirm swap & ram
free -g

# create swap file
sudo fallocate -l 8G /swap_file

# add correct permissions for it
sudo chmod 600 /swap_file

# enable swap area on swap file
sudo mkswap /swap_file

# add swap file entry in fstab file
echo "swap_file  swap   swap   defaults   0 0" | sudo tee -a /etc/fstab

# extend / enable extra swap space
sudo swapon /swap_file

# now confirm if swap has been extended
free -g

# check the multiple swap
swapon -s
```

Note: This is the option I am currently using to see if it would work differently from the first one.

## References

- [How To Check Swap Usage Size and Utilization in Linux](https://www.cyberciti.biz/faq/linux-check-swap-usage-command/)
- [How to Extend Swap Space using Swap file in Linux](https://www.linuxtechi.com/extend-swap-space-using-swap-file-in-linux/)
- [VMware Player warns me of no swap whenever I launch a VM](https://askubuntu.com/questions/449643/vmware-player-warns-me-of-no-swap-whenever-i-launch-a-vm)
