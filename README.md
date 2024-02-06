# tumbleweed-update
An automated script to perform update tasks on Opensuse Tumbleweed using zypper and flatpak, allowing for GPUcache cleanup

I made this thing some time ago, i use it every couple days to update my machine. The script refreshes zypper, performs "dup" update as well as updates all the flatpaks. At the end it asks if you want to clean the gpucache ( it is important, as I had many issues with Google Chrome not displaying graphics correctly after TW updates,.Using my script after updates, before rebooting, resolved those issues completely). At the end it asks if you want to reboot.

The script is just 3 update commands and two while loops - one for choice of gpucache cleaning and one for reboot choice. It also has a command that finds any GPUcache folders in ~/ and deletes them, so they can regenerate after reboot properly. I have it in my ~/bin folder named "myupdate", so every time i execute "myupdate" command in the terminal, it takes care of everything.

I did NOT implement non-interactive zypper or flatpak updates, just as a safety measure, so in case you spot a bad update or bad vendor, you can cancel or abort it and interact with zypper or flatpak.

If you want the non-interactive setup, simply change "sudo zypper refresh && sudo zypper dup && sudo flatpak update" to "sudo zypper refresh && sudo zypper --non-interactive dup && sudo flatpak update --noninteractive"

Here are the contents of the bash script fiile:
```bash
#!/bin/bash

sudo zypper refresh && sudo zypper dup && sudo flatpak update

while true; do
    read -p "Do you want to clean the GPU cache? (y/n): " answer

    case $answer in
        [Yy]* )
            echo "Cleaning GPU cache..."
            sudo find ~/ -type d -name GPUCache -exec rm -rf {} +
            break
            ;;
        [Nn]* )
            echo "Exiting without cleaning the GPU cache."
            break
            ;;
        * )
            echo "Invalid response. Please answer y or n."
            continue
            ;;
    esac
done

while true; do
    read -p "Do you want to reboot now? (y/n): " reboot_answer

    case $reboot_answer in
        [Yy]* )
            echo "Rebooting now..."
            sudo reboot
            break
            ;;
        [Nn]* )
            echo "Exiting without rebooting."
            break
            ;;
        * )
            echo "Invalid response. Please answer y or n."
            continue
            ;;
    esac
done
```


You can just do "nano ~/bin/twupdate" and paste the contents into it. afterwards "chmod +x ~/bin/twupdate" and you are ready to roll.

Hope someone finds it useful!
