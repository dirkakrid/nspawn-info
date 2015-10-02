# nspawn-info
systemd-nspawn setup/configuration/information for usage/integration (opensuse tumbleweed). The wrapper 
script provides the general structure of performing the following and is reasonable starting point

## Using the script
* Download the nspawn script to preferred location and update the placeholders

| Name             | URI                                                     |
|------------------|---------------------------------------------------------|
| {CONTAINERS}     | The path where containers should be stored              |
| {PATH_TO_SCRIPT} | Path where the nspawn script is stored (directory only) |
| {SHARED}         | A path to share with the containers                     |

## Install more packages (template)
```text
nspawn install template package1 package2...
```

## Storage (template)
```text
nspawn pack template
```

## Unpack (template)
```text
nspawn unpack template
```

## Start (not boot)
```text'
nspawn start template
```

## Start (boot with network)
```text
nspawn boot template
```

## Instance (from unpacked template)
```text
nspawn clone copy-of-template
```

## Setup/install
* Run a script (sort of like this) and it should be all set
```text
NSPAWN_FILE="/path/to/output/file/for/nspawn
NSPAWN_URL="https://raw.githubusercontent.com/enckse/nspawn-info/master/nspawn"
CONTAINERS="\/path\/to\/containers\/"
SHARED="\/path\/to\/shared"
PATHTO="\/path\/to\/directory\/"
wget -qO- $NSPAWN_URL | sed -e "s/{CONTAINERS}/$CONTAINERS/g" | sed -e "s/{PATH_TO_SCRIPT}/$PATHTO/g" | sed -e "s/{SHARED}/$SHARED/g" > $NSPAWN_FILE
```

## Bash auto-completion
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


### Notes
* Create the 'base' container (template) using these 'basic' steps:
```text
mkdir  /absolute/path/to/template

# For each 'necessary' repository add it
zypper --root /absolute/path/to/template/ ar -f -c {url} {name}

# Install the packages for a base container
zypper --root /absolute/path/to/template/ in patterns-openSUSE-64bit patterns-openSUSE-base zypper rpm
```

* Create the 'base' container (template) for tumbleweed using these repos:

| Name         | URI                                                    |
|--------------|--------------------------------------------------------|
| repo-non-oss | http://download.opensuse.org/tumbleweed/repo/non-oss   |
| repo-oss     | http://download.opensuse.org/tumbleweed/repo/oss       |
| repo-debug   | http://download.opensuse.org/tumbleweed/repo/debug     |
| repo-update  | http://download.opensuse.org/update/tumbleweed         |


* Create the 'base' container (template) for 13.2 using these repos:

| Name                | URI                                                           |
|---------------------|---------------------------------------------------------------|
| repo-non-oss        | http://download.opensuse.org/distribution/13.2/repo/non-oss/  |
| repo-oss            | http://download.opensuse.org/distribution/13.2/repo/oss/      |
| repo-update         | http://download.opensuse.org/update/13.2/                     |
| repo-update-non-oss | http://download.opensuse.org/update/13.2-non-oss/             |

* Share X with the container via this (in the nspawn script, disable sharing X from the host by changing ENABLE_X to != 1):
```text
# host
xhost +local: > /dev/null 2>&1

# container
export DISPLAY=:0
```
* Booting debian in another distro
```text
# Using debootstrap
zypper in debootstrap
debootstrap --arch=amd64 unstable debian/
systemd-nspawn -D debian/

# Full system in the container
systemd-nspawn -D debian-tree/ /sbin/init
```

* Enable root access (disable by default in openSUSE)
```text
echo console >> /etc/securetty
sed -i 's/session\s*required\s*pam_loginuid.so/#session    required     pam_loginuid.so/' /etc/pam.d/login
```

## Going forward
* Currently this is used mainly in 13.2 (for stability) which restricts it somewhat
* Hopefully, once the next major (stable) version is released options like overlayfs would be available (to be explored in the future)

## Useful references:
[1] https://wiki.archlinux.org/index.php/Systemd-nspawn

[2] http://rorgoroth.blogspot.com/2014/12/setting-up-bare-opensuse-systemd-nspawn.html

[3] http://0pointer.de/blog/projects/changing-roots.html

[4] http://www.do-not-panic.com/2014/08/hacking-on-systemd-with-opensuse.html

[5] https://en.opensuse.org/User:Tsu2/chroot-nspawn-OpenStack
