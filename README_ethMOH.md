# README_ethMOH

## branches/ethMOH

* Hardware - IR077E
* MCU - MT7688A

## Shell

```sh
git clone https://github.com/hnhkj/CC15.05.git -b ethMOH --depth=1
cd CC15.05
cp config.diff .config
$make defconfig
make j=4
```

#### .config

```sh
./scripts/diffconfig.sh > config.diff
cp config.diff .config
make defconfig
```

## ASK

#### make menuconfig

*ERR-01*

```sh
Checking 'ldconfig-stub'... ok.

Build dependency: Please install Git (git-core) >= 1.6.5

/home/ubuntu/ethMOH/os/openwrt_cc15.05/include/prereq.mk:12: recipe for target 'prereq' failed
Prerequisite check failed. Use FORCE=1 to override.
/home/ubuntu/ethMOH/os/openwrt_cc15.05/include/toplevel.mk:140: recipe for target 'staging_dir/host/.prereq-build' failed
make: *** [staging_dir/host/.prereq-build] Error 1

```
