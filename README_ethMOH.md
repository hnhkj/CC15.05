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
make V=s j=4
```

#### .config

```sh
./scripts/diffconfig.sh > config.diff
cp config.diff .config
make defconfig
```

## ASK

#### 1.0 make menuconfig - git version

*ERROR*

```sh
Checking 'ldconfig-stub'... ok.

Build dependency: Please install Git (git-core) >= 1.6.5

/home/ubuntu/ethMOH/os/openwrt_cc15.05/include/prereq.mk:12: recipe for target 'prereq' failed
Prerequisite check failed. Use FORCE=1 to override.
/home/ubuntu/ethMOH/os/openwrt_cc15.05/include/toplevel.mk:140: recipe for target 'staging_dir/host/.prereq-build' failed
make: *** [staging_dir/host/.prereq-build] Error 1

```

*modify*

see <https://github.com/openwrt/openwrt/commit/4c80909fa141fe2921c62bd17b2b04153031df18>

```sh
$ git diff include/
diff --git a/include/prereq-build.mk b/include/prereq-build.mk
index 32c4adabb7..e79a727b0e 100644
--- a/include/prereq-build.mk
+++ b/include/prereq-build.mk
@@ -145,7 +145,7 @@ $(eval $(call SetupHostCommand,svn,Please install the Subversion client, \
        svn --version | grep Subversion))
 
 $(eval $(call SetupHostCommand,git,Please install Git (git-core) >= 1.6.5, \
-       git clone 2>&1 | grep -- --recursive))
+       git –exec-path | xargs -I % – grep -q – –recursive %/git-submodule))
```