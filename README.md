# nspawn-info
systemd-nspawn setup/configuration/information for usage/integration. The wrapper 
script provides the general structure of performing the following and is reasonable starting point

### Using the script
* Download the nspawn script and check that all environment variables are set as preferred

### Setup/install
* Run a script (sort of like this) and it should be all set
```text
NSPAWN_FILE="/path/to/output/file/for/nspawn"
NSPAWN_URL="https://raw.githubusercontent.com/enckse/nspawn-info/master/nspawn"
wget -qO- $NSPAWN_URL > $NSPAWN_FILE
```

### Bash auto-completion
* Use the 'nspawn_autocompletion' for bash auto completion in /etc/bash_completion.d/

### X (sharing)
* Share X with the container via this
```text
# host
xhost +local: > /dev/null 2>&1

# container
export DISPLAY=:0
```

### Allowing root login
* Per the arch wiki if, at login, root can't login
```
# again, check the arch wiki to validate this
rm /etc/securetty
```

### Screen
* Machines are spawned via screen but that can be disabled by setting the environment variable NSPAWN_INFO_SCREEN != 1

### Using debootstrap
* Booting debian in another distro
```text
# Using debootstrap
debootstrap --arch=amd64 unstable debian/
```

### Using pacstrap
* Loading with pacstrap
```
cd /path/to/containers
sudo pacstrap -i -c -d template/ base
```

### Updating
* Just 'arch-chroot' in and call pacman
```
arch-chroot /path/to/container /bin/bash -c 'pacman Syyu'
```

### Config file
* Storing a config file in $HOME/.config/.nspawn-info can provide some overrides for the user while providing a .nspawn-info in the container directory will be system-wide (but overriden by any user settings)
```
[machine_name].arguments=--bind /var/log
```

### Useful references:
[1] https://wiki.archlinux.org/index.php/Systemd-nspawn

[2] http://rorgoroth.blogspot.com/2014/12/setting-up-bare-opensuse-systemd-nspawn.html

[3] http://0pointer.de/blog/projects/changing-roots.html

[4] http://www.do-not-panic.com/2014/08/hacking-on-systemd-with-opensuse.html

[5] https://en.opensuse.org/User:Tsu2/chroot-nspawn-OpenStack
