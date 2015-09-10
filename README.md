# nspawn-info
systemd-nspawn setup/configuration/information for usage/integration (opensuse tumbleweed). The wrapper 
script provides the general structure of performing the following and is reasonable starting point

## Creation (template)
```text
./nspawn-wrapper create template
```

## Install more packages (template)
```text
./nspawn-wrapper install template package1 package2...
```

## Storage (template)
```text
./nspawn-wrapper pack template
```

## Unpack (template)
```text
./nspawn-wrapper unpack template
```

## Start (not boot)
```text'
./nspawn-wrapper start template
```


## Start (boot with network)
```text
./nspawn-wrapper boot template
```

## Instance (from unpacked template)
```text
./nspawn-wrapper clone copy-of-template
```

### Notes
* Share X with the container via this:
```text
# host
xhost +local: > /dev/null 2>&1

# container
export DISPLAY=:0
```

## Useful references:
[1] https://wiki.archlinux.org/index.php/Systemd-nspawn

[2] http://rorgoroth.blogspot.com/2014/12/setting-up-bare-opensuse-systemd-nspawn.html
