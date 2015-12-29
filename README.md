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
* Add the following as a script at "/etc/bash_completion.d/nspawn"
```text
#!/bin/bash
_nspawn()
{
    local cur prev opts idx first
    cur=${COMP_WORDS[COMP_CWORD]}
    prev=${COMP_WORDS[COMP_CWORD-1]}
    if [ $COMP_CWORD -eq 1 ]; then
        opts=$(nspawn operations)
        COMPREPLY=( $(compgen -W "$opts" -- $cur) )
    else
    first="false"
        for idx in $(nspawn options); do
            if [[ $prev == $idx ]]; then
                first="true"
            fi
        done
        if [[ $first == "false" ]]; then
            opts=$(nspawn list) 
            COMPREPLY=( $(compgen -W "$opts" -- $cur) )
        fi
    fi
}
complete -F _nspawn nspawn
```

### X (sharing)
* Share X with the container via this (in the nspawn script, disable sharing X from the host by changing the environment to NSPAWN_INFO_XHOST != 1):
```text
# host
xhost +local: > /dev/null 2>&1

# container
export DISPLAY=:0
```

### Screen
* Machines are spawned via screen but that can be disabled by setting the environment variable NSPAWN_INFO_SCREEN != 1

### Using debootstrap
* Booting debian in another distro
```text
# Using debootstrap
debootstrap --arch=amd64 unstable debian/
```

### Config file
* Storing a config file in $HOME/.config/.nspawn-info can provide some overrides for the user while providing a .nspawn-info in the container directory will be system-wide (but overriden by any user settings)
```
[machine_name].overlay=1
[machine_name].arguments="--bind /var/log"
```
Currently overlay will allow for disabling/enabling overlay fs for a specific machine and override the NSPAWN_INFO_OVERLAY flag. The 'arguments' setting will be passed to systemd-nspawn as additional arguments at boot

### Useful references:
[1] https://wiki.archlinux.org/index.php/Systemd-nspawn

[2] http://rorgoroth.blogspot.com/2014/12/setting-up-bare-opensuse-systemd-nspawn.html

[3] http://0pointer.de/blog/projects/changing-roots.html

[4] http://www.do-not-panic.com/2014/08/hacking-on-systemd-with-opensuse.html

[5] https://en.opensuse.org/User:Tsu2/chroot-nspawn-OpenStack
