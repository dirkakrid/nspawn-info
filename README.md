# nspawn-info
systemd-nspawn setup/configuration/information for usage/integration (opensuse tumbleweed)

## Creation (template)
```text
PATHTO=/absolute/path/to/template/
zypper --root $PATHTO ar -f -c http://download.opensuse.org/tumbleweed/repo/oss repo-oss
zypper --root $PATHTO ar -f -c http://download.opensuse.org/tumbleweed/repo/non-oss repo-non-oss
zypper --root $PATHTO ar -f -c http://download.opensuse.org/tumbleweed/repo/debug repo-debug
zypper --root $PATHTO ar -f -c http://download.opensuse.org/update/tumbleweed repo-update
zypper --root $PATHTO in patterns-openSUSE-64bit patterns-openSUSE-base zypper rpm
```

## Install more packages (template)
```text
PATHTO=/absolute/path/to/template/
zypper --root $PATHTO in git nano
```

## Storage (template)
```text
tar -cvpzf template.tar.gz --one-file-system template/
```

## Unpack (template)
```text
tar xzvf template.tar.gz
```

## Start (not boot)
```text
systemd-nspawn -D template/
```

## Start (boot with network)
```text
systemd-nspawn -b -D template/ -n
```

## Instance (from unpacked template)
```text
NAMED="name"
PATHTO=/absolute/path/to/instance/$NAMED/
if [ -d $PATHTO ]; then
    rm -rf $PATHTO
fi

cp -R template $NAMED
zypper --root $PATHTO in git nano
```

## Useful references:
1 https://wiki.archlinux.org/index.php/Systemd-nspawn
2 http://rorgoroth.blogspot.com/2014/12/setting-up-bare-opensuse-systemd-nspawn.html
