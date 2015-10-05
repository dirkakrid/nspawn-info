# nspawn-info
systemd-nspawn setup/configuration/information for usage/integration. The wrapper 
script provides the general structure of performing the following and is reasonable starting point

### Using the script
* Download the nspawn script to preferred location and update the placeholders

| Name             | URI                                                     |
|------------------|---------------------------------------------------------|
| {CONTAINERS}     | The path where containers should be stored              |
| {PATH_TO_SCRIPT} | Path where the nspawn script is stored (directory only) |
| {SHARED}         | A path to share with the containers                     |

### Setup/install
* Run a script (sort of like this) and it should be all set
```text
NSPAWN_FILE="/path/to/output/file/for/nspawn"
NSPAWN_URL="https://raw.githubusercontent.com/enckse/nspawn-info/master/nspawn"
CONTAINERS="\/path\/to\/containers\/"
SHARED="\/path\/to\/shared"
PATHTO="\/path\/to\/directory\/"
wget -qO- $NSPAWN_URL | sed -e "s/{CONTAINERS}/$CONTAINERS/g" | sed -e "s/{PATH_TO_SCRIPT}/$PATHTO/g" | sed -e "s/{SHARED}/$SHARED/g" > $NSPAWN_FILE
```

### Packing a template
* Pack a template using tar/gz
```text
tar -cvpzf $TARFILE --one-file-system "$OBJPATH/"
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
* Share X with the container via this (in the nspawn script, disable sharing X from the host by changing ENABLE_X to != 1):
```text
# host
xhost +local: > /dev/null 2>&1

# container
export DISPLAY=:0
```

### Using debootstrap
* Booting debian in another distro
```text
# Using debootstrap
debootstrap --arch=amd64 unstable debian/
```

### Useful references:
[1] https://wiki.archlinux.org/index.php/Systemd-nspawn

[2] http://rorgoroth.blogspot.com/2014/12/setting-up-bare-opensuse-systemd-nspawn.html

[3] http://0pointer.de/blog/projects/changing-roots.html

[4] http://www.do-not-panic.com/2014/08/hacking-on-systemd-with-opensuse.html

[5] https://en.opensuse.org/User:Tsu2/chroot-nspawn-OpenStack
