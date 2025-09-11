1. VBoxLinuxAdditions
```
sudo apt update
sudo apt install build-essential dkms linux-headers-$(uname -r)

sudo sh /media/$USER/VBox*/VBoxLinuxAdditions.run
```

2. Shared Folders 
Fix: Add Your User to vboxsf Group
Inside your guest OS (Linux) run:
```
sudo usermod -aG vboxsf $USER
```

3. Update the ubuntu-linux through **SoftwareUpdater**
