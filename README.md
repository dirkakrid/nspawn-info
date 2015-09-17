# nspawn-info
systemd-nspawn setup/configuration/information for usage/integration (opensuse tumbleweed). The wrapper 
script provides the general structure of performing the following and is reasonable starting point

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

### Notes
* Create the 'base' container (template) for tumbleweed:
```text
mkdir  /absolute/path/to/template

# For each 'necessary' repository add it
zypper --root /absolute/path/to/template/ ar -f -c {url} {name}

# Install the packages for a base container
zypper --root /absolute/path/to/template/ in patterns-openSUSE-64bit patterns-openSUSE-base zypper rpm
```

* Create the 'base' container (template) for tumbleweed using these repos:
Alias               | Name         | URI
--------------------|--------------|--------------------------------------------------------------
repo-non-oss        | repo-non-oss | http://download.opensuse.org/tumbleweed/repo/non-oss
repo-oss            | repo-oss     | http://download.opensuse.org/tumbleweed/repo/oss
repo-debug          | repo-debug   | http://download.opensuse.org/tumbleweed/repo/debug
repo-update         | repo-update  | http://download.opensuse.org/update/tumbleweed


* Create the 'base' container (template) for 13.2 using these repos:
Alias               | Name                | URI
--------------------|---------------------|--------------------------------------------------------------
repo-non-oss        | repo-non-oss        | http://download.opensuse.org/distribution/13.2/repo/non-oss/
repo-oss            | repo-oss            | http://download.opensuse.org/distribution/13.2/repo/oss/
repo-update         | repo-update         | http://download.opensuse.org/update/13.2/
repo-update-non-oss | repo-update-non-oss | http://download.opensuse.org/update/13.2-non-oss/

* Share X with the container via this:
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

## Useful references:
[1] https://wiki.archlinux.org/index.php/Systemd-nspawn

[2] http://rorgoroth.blogspot.com/2014/12/setting-up-bare-opensuse-systemd-nspawn.html

[3] http://0pointer.de/blog/projects/changing-roots.html

[4] http://www.do-not-panic.com/2014/08/hacking-on-systemd-with-opensuse.html
