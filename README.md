# Manifest for building CrDroid 13.0 for gtaxllte

`gtaxllte` is the codename for the LTE variant of the Samsung Galaxy Tab A 10.1" (2016), with model SM-T585, and also with SM-T585N0 and SM-T585C for Korea and China respectively.

Some extremely basic instructions:

These assume you have a good Linux build environment prepared with all prerequisite dependencies installed. If not, you should be able to find out how to prepare a build environment, and what dependencies you should have installed for your specific Linux distribution, by searching elsewhere.

- Make a new directory for CrDroid 13.0 sources and enter it:
```
mkdir crDroid
cd crDroid
```

- Initialize repo in this directory with CrDroid's android.git repository:
```
repo init -u https://github.com/crdroidandroid/android.git -b 13.0 --git-lfs
```

- Clone this repository to .repo/local_manifests for the manifest, gtaxl.xml, containing the repositories needed to build for this device:
```
git clone https://github.com/SeifHossam17/gtaxl-manifests.git -b crdroid-13.0-gtaxllte .repo/local_manifests
```

- Sync all of the repositories in manifests (including LineageOS manifests):
```
repo sync --force-sync --no-tags --no-clone-bundle -c
```

- Forks from LineageOS repositories may become out-of-date from new changes until I (@K9100ii) get around to updating them again. Before building, you'll need to go through gtaxl.xml, and, for forks that have remove-project lines with names starting with "LineageOS/", update (rebase) them from upstream repositories. For example, for frameworks/base (note the second and third commands only need to be ran once per repository):
```
cd frameworks/base
git remote add lineage https://github.com/LineageOS/android_frameworks_base
git config pull.rebase true
git pull lineage lineage-20.0
cd ../..
```

- Finally, build as you like. For example, for a recovery-installable package for gtaxllte:
```
. build/envsetup.sh
lunch lineage_gtaxllte-userdebug
mka otapackage
```
