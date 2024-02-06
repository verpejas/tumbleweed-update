# tumbleweed-update
An automated script to perform update tasks on Opensuse Tumbleweed using zypper and flatpak, allowing for GPUcache cleanup

I made this thing some time ago, i use it every couple days to update my machine. The script refreshes zypper, performs "dup" update as well as updates all the flatpaks. At the end it asks if you want to clean the gpucache (i had many issues with google chrome after TW updates, using this after update before rebooting resolved those issues). At the end it asks for a reboot.

The script is just 3 update commands and two while loops - one for choice of gpucache cleaning and one for reboot choice. I have it in my ~/bin folder named "myupdate", so every time i execute "myupdate" command in the terminal, it takes care of everything for me.

I did not implement non-interactive zypper updates, just as a safety measure, so if you can spot a bad update or bad vendor you can cancel or abort it and interact with zypper. Same goes for flatpaks

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
