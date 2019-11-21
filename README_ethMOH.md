# README_ethMOH

## CC15.05/ethMOH - working...

* Hardware - IR077E
* MCU - MT7688A

## Shell

```sh
sudo apt-get install git g++ make libncurses5-dev subversion libssl-dev gawk libxml-parser-perl unzip wget python xz-utils
git clone https://github.com/hnhkj/CC15.05.git -b ethMOH --depth=1
cd CC15.05

cp feeds.conf.default feeds.conf
./scripts/feeds update
./scripts/feeds install -a

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

#### Changed

```
openwrt/package/ethMOH/*
openwrt/package/mtk-sdk-wifi/*
```

## FAQ

#### 1. git version error when make menuconfig

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

#### 2. gdate.c:2497:7: error: format not a string literal, format string not checked [-Werror=format-nonlit

create files `tools/pkg-config/patches/001-glib-gdate-suppress-string-format-literal-warning.patch` and copy follow information to file.

```sh
--- a/glib/glib/gdate.c
+++ b/glib/glib/gdate.c
@@ -2439,6 +2439,9 @@ win32_strftime_helper (const GDate     *d,
  *
  * Returns: number of characters written to the buffer, or 0 the buffer was too small
  */
+#pragma GCC diagnostic push
+#pragma GCC diagnostic ignored "-Wformat-nonliteral"
+
 gsize     
 g_date_strftime (gchar       *s, 
                  gsize        slen, 
@@ -2549,3 +2552,5 @@ g_date_strftime (gchar       *s,
   return retval;
 #endif
 }
+
+#pragma GCC diagnostic pop
```

#### 3. Unescaped left brace in regex is illegal here in regex; marked by <-- HERE in m/\${ <-- HERE ([^ \t=:+{}]+)}/ at ./bin/automake.tmp line 393

see <https://blog.csdn.net/rainforest_c/article/details/82722198>

create a patch `tools/automake/patches/210-fix-bug-becuase-of-high-perl-version.patch`

```sh
diff --git a/bin/automake.in b/bin/automake.in
index 0ee37149dd..8ce621d1af 100644
--- a/bin/automake.in
+++ b/automake.in
@@ -3880,7 +3880,8 @@ sub substitute_ac_subst_variables_worker
 sub substitute_ac_subst_variables
 {
   my ($text) = @_;
-  $text =~ s/\${([^ \t=:+{}]+)}/substitute_ac_subst_variables_worker ($1)/ge;
+#  $text =~ s/\${([^ \t=:+{}]+)}/substitute_ac_subst_variables_worker ($1)/ge;
+  $text =~ s/\$[{]([^ \t=:+{}]+)}/substitute_ac_subst_variables_worker ($1)/ge;
   return $text;
 }
```

#### 4. fatal error: linux/compiler-gcc7.h: No such file or directory

复制 <https://github.com/hnhkj/chaos_calmer.git> `tools/mkimage/patches/200-compiler-support.patch` 到相应的目录打补丁。


#### fbtft LCD

<https://github.com/notro/fbtft>


#### 获取之前的提交信息

```sh
git pull --unshallow
```
