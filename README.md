# wandboard-imx6-yocto-zeus_5.4

https://www.technexion.com/products/system-on-modules/evk/wandboard-imx6/

<img title="a title" alt="Alt text" src="/img/imx6-yocto-zeus-5.png">

<img title="a title" alt="Alt text" src="/img/imx6-yocto-zeus-4.png">

following TechNexion Yocto 3.0 Zeus 5.4.y GA BSP 

https://github.com/TechNexion/tn-imx-yocto-manifest/tree/zeus_5.4.y-next

** Work By VM VirsualBox Ubuntu 18.04.6 **

** Docker Try Prove but build fail **

----------------------------------------

Downloading Repo source from https://gerrit.googlesource.com/git-repo
fatal: unable to access 'https://gerrit.googlesource.com/git-repo/': Problem with the SSL CA cert (path? access rights?)
repo: error: "git" failed with exit status 128
  cwd: /workspaces/ws-techNexion-dev-docker/emd_yocto/.repo/repo.tmp
  cmd: ['git', 'fetch', '--quiet', '--progress', 'origin', '+refs/heads/*:refs/remotes/origin/*', '+refs/tags/*:refs/tags/*']
fatal: double check your --repo-rev setting.
fatal: cloning the git-repo repository failed, will remove '.repo/repo' 

Prove By : git config --global --list

http.sslcainfo=D:2024.1/tps/win64/git-2.16.2/mingw64/ssl/certs/ca-bundle.crt <-- Incorrect.

Fixed : git config --global http.sslCAInfo /etc/ssl/certs/ca-certificates.crt

--------------------------------------

Cloning into bare repository '/workspaces/ws-techNexion-dev-docker/emd_yocto/downloads//git2/github.com.nxp-imx.linux-imx.git'...
remote: Enumerating objects: 13281991, done.        
remote: Counting objects: 100% (22296/22296), done.        
remote: Compressing objects: 100% (2534/2534), done.        
error: RPC failed; curl 56 GnuTLS recv error (-54): Error in the pull function.
fatal: the remote end hung up unexpectedly
fatal: early EOF
fatal: index-pack failed

ERROR: linux-imx-headers-5.4-r0 do_fetch: Fetcher failure for URL: 'git://github.com/nxp-imx/linux-imx.git;protocol=https;branch=imx_5.4.70_2.3.0'. Unable to fetch URL from any source.
ERROR: Logfile of failure stored in: /workspaces/ws-techNexion-dev-docker/emd_yocto/build-x11-wandboard-imx6/tmp/work/cortexa9t2hf-neon-mx6qdl-poky-linux-gnueabi/linux-imx-headers/5.4-r0/temp/log.do_fetch.94456
ERROR: Task (/workspaces/ws-techNexion-dev-docker/emd_yocto/sources/meta-imx/meta-bsp/recipes-kernel/linux/linux-imx-headers_5.4.bb:do_fetch) failed with exit code '1'

Fixed by : sudo apt update && sudo apt install openssl curl gnutls-bin 
    bitbake -c cleansstate imx-image-full
    bitbake imx-image-full

Option : https://stackoverflow.com/questions/69332393/yocto-error-in-building-an-image-using-bitbake

--------------------------------------------------------------------------------------------------------

Exception: subprocess.CalledProcessError: Command 'cp -afl --preserve=xattr ./* /workspaces/ws-techNexion-dev-docker/emd_yocto/build-x11-wandboard-imx6/tmp/sysroots-components/x86_64/icu-native' returned non-zero exit status 1.

Subprocess output:
cp: cannot create hard link '/workspaces/ws-techNexion-dev-docker/emd_yocto/build-x11-wandboard-imx6/tmp/sysroots-components/x86_64/icu-native/usr/lib/icu/current' to './usr/lib/icu/current': Is a directory

ERROR: Logfile of failure stored in: /workspaces/ws-techNexion-dev-docker/emd_yocto/build-x11-wandboard-imx6/tmp/work/x86_64-linux/icu-native/64.2-r0/temp/log.do_populate_sysroot.10342
ERROR: Task (virtual:native:/workspaces/ws-techNexion-dev-docker/emd_yocto/sources/poky/meta/recipes-support/icu/icu_64.2.bb:do_populate_sysroot) failed with exit code '1'

----

DISPLAY=hdmi WIFI_FIRMWARE=y WIFI_MODULE=qca DISTRO=fsl-imx-x11 MACHINE=wandboard-imx6 source tn-setup-release.sh -b build-x11-wandboard-imx6

--------

Cloning into bare repository '/home/jenkins/edm_yocto/downloads//git2/github.com.nxp-imx.linux-imx.git'...
remote: Enumerating objects: 13281991, done.
remote: Counting objects: 100% (22296/22296), done.
remote: Compressing objects: 100% (2534/2534), done.
error: RPC failed; curl 56 GnuTLS recv error (-9): A TLS packet with unexpected length was received.
fatal: The remote end hung up unexpectedly
fatal: early EOF
fatal: index-pack failed

Fixed by : 

https://stackoverflow.com/questions/69332393/yocto-error-in-building-an-image-using-bitbake

sudo apt update && sudo apt install openssl curl gnutls-bin

git config --global http.postBuffer 524288000

git config --global https.postBuffer 524288000

git config --global core.compression -1

Prove By : git config --global --list

user.email=<xxx@xxxmail.com>
user.name=<User>
color.ui=auto
http.postbuffer=524288000
http.maxrequestbuffer=100M
core.compression=-1
https.postbuffer=524288000

option 1 : https://github.com/orgs/community/discussions/134430

-------------------------

repo: Updating release signing keys to keyset ver 2.3
Traceback (most recent call last):
  File "/home/jenkins/bin/repo", line 1432, in <module>
    main(sys.argv[1:])
  File "/home/jenkins/bin/repo", line 1382, in main
    _Init(args)
  File "/home/jenkins/bin/repo", line 644, in _Init
    os.rename(dst, dst_final)
PermissionError: [Errno 13] Permission denied: '/home/jenkins/edm_yocto/.repo/repo.tmp' -> '/home/jenkins/edm_yocto/.repo/repo'

Fixed by : PATH=${PATH}:~/bin 

--------------------------

docker build -t tn-develop-ubuntu .

docker run -it -u jenkins -v "$(pwd)/workspaces:/home/jenkins" tn-develop-ubuntu bash

----------------------------

WARNING: bmap-tools-native-3.5+gitAUTOINC+db7087b883-r0 do_fetch: Failed to fetch URL git://github.com/intel/bmap-tools, attempting MIRRORS if available
ERROR: bmap-tools-native-3.5+gitAUTOINC+db7087b883-r0 do_fetch: Fetcher failure: Unable to find revision db7087b883bf52cbff063ad17a41cc1cbb85104d in branch master even from upstream
ERROR: bmap-tools-native-3.5+gitAUTOINC+db7087b883-r0 do_fetch: Fetcher failure for URL: 'git://github.com/intel/bmap-tools'. Unable to fetch URL from any source.
ERROR: Logfile of failure stored in: /home/vboxuser/Desktop/ws-tn-linux-yocto/edm_yocto/build-x11-wandboard-imx6/tmp/work/x86_64-linux/bmap-tools-native/3.5+gitAUTOINC+db7087b883-r0/temp/log.do_fetch.17363
ERROR: Task (virtual:native:/home/vboxuser/Desktop/ws-tn-linux-yocto/edm_yocto/sources/poky/meta/recipes-support/bmap-tools/bmap-tools_3.5.bb:do_fetch) failed with exit code '1'

Fixed by : https://lists.openembedded.org/g/openembedded-core/message/179666

-----------------------------

cp -L imx-image-full-wandboard-imx6.wic.bz2  /media/sf_shareFolder/tn-linux/

------------------------------

NOTE: recipe qt3d-5.15.0+gitAUTOINC+d1e6bd43de-r0: task do_compile: Failed

Option 1 : bitbake qt3d -c compile -f

------------------------------

ERROR: puzzles-2_0.0+gitAUTOINC+c6e0161dd4-r0 do_fetch: Fetcher failure: Unable to find revision c6e0161dd475415316ed66dc82794d68e52f0025 in branch master even from upstream
ERROR: puzzles-2_0.0+gitAUTOINC+c6e0161dd4-r0 do_fetch: Fetcher failure for URL: 'git://git.tartarus.org/simon/puzzles.git'. Unable to fetch URL from any source.
ERROR: Logfile of failure stored in: /home/vboxuser/Desktop/ws-tn-linux-yocto/edm_yocto/build-x11-wandboard-imx6/tmp/work/cortexa9t2hf-neon-poky-linux-gnueabi/puzzles/2_0.0+gitAUTOINC+c6e0161dd4-r0/temp/log.do_fetch.29347
NOTE: recipe puzzles-2_0.0+gitAUTOINC+c6e0161dd4-r0: task do_fetch: Failed
ERROR: Task (/home/vboxuser/Desktop/ws-tn-linux-yocto/edm_yocto/sources/poky/meta/recipes-sato/puzzles/puzzles_git.bb:do_fetch) failed with exit code '1'
NOTE: Running task 4267 of 9329 (/home/vboxuser/Desktop/ws-tn-linux-yocto/edm_yocto/sources/poky/meta/recipes-kernel/blktrace/blktrace_git.bb:do_fetch)

Renamed default branch
For the time being, the community is following the approach of changing the default branch name of the git repositories from 'master' to 'main'. The new default branch name is understood as being more descriptive and inclusive, as, e.g. referenced by this article: https://about.gitlab.com/blog/2021/03/10/new-git-default-branch-name/#a-more-descriptive-and-inclusive-name

ERROR: googletest-1.10.0-r0 do_fetch: Fetcher failure: Unable to find revision 703bd9caab50b139428cea1aaff9974ebee5742e in branch master even from upstream
ERROR: googletest-1.10.0-r0 do_fetch: Fetcher failure for URL: 'git://github.com/google/googletest.git'. Unable to fetch URL from any source.
ERROR: Logfile of failure stored in: /home/vboxuser/Desktop/ws-tn-linux-yocto/edm_yocto/build-x11-wandboard-imx6/tmp/work/cortexa9t2hf-neon-poky-linux-gnueabi/googletest/1.10.0-r0/temp/log.do_fetch.17827
NOTE: recipe googletest-1.10.0-r0: task do_fetch: Failed
ERROR: Task (/home/vboxuser/Desktop/ws-tn-linux-yocto/edm_yocto/sources/meta-imx/meta-bsp/recipes-test/googletest/googletest_git.bb:do_fetch) failed with exit code '1'
NOTE: Running task 2973 of 9329 (/home/vboxuser/Desktop/ws-tn-linux-yocto/edm_yocto/sources/meta-openembedded/meta-oe/recipes-devtools/rapidjson/rapidjson_git.bb:do_fetch)

S = "${WORKDIR}/git"
SRCREV = "703bd9caab50b139428cea1aaff9974ebee5742e"
-SRC_URI = "git://github.com/google/googletest.git;branch=master;protocol=https"
+SRC_URI = "git://github.com/google/googletest.git;branch=main;protocol=https"

https://patchwork.yoctoproject.org/project/oe/patch/9B6z.1699621499757734697.wXhd@lists.openembedded.org/#14837

------------------------------

## Insert SD-Card 

<img title="a title" alt="Alt text" src="/img/imx6-yocto-zeus-3.png">

## Try Booting

<img title="a title" alt="Alt text" src="/img/imx6-yocto-zeus-2.png">

<img title="a title" alt="Alt text" src="/img/imx6-yocto-zeus-1.png">

