## Install android build dependencies 

```bash
curl https://raw.githubusercontent.com/akhilnarang/scripts/master/setup/android_build_env.sh -o android_build_env.sh
chmod +x android_build_env.sh
./android_build_env.sh
sudo apt-get install python3-pip
sudo pip install lxml
```

## Initialize Evolution X via repo and sync source

```bash
mkdir evolution && cd evolution
repo init -u https://github.com/Evolution-XYZ/manifest -b udc --git-lfs
repo sync -c -j$(nproc --all) --force-sync --no-clone-bundle --no-tags
```

**Note:** If repo sync fails to fully sync the tree (likely due to rate limiting), re-run the repo init & sync commands again.

## Set up build environment
```bash
. build/envsetup.sh
```

## Lunch your target device

| Codename   | Device       |
|------------|--------------|
| oriole     | Pixel 6      |
| raven      | Pixel 6 Pro  |
| bluejay    | Pixel 6a     |
| panther    | Pixel 7      |
| cheetah    | Pixel 7 Pro  |
| lynx       | Pixel 7a     |
| tangorpro  | Pixel Tablet |
| felix      | Pixel Fold   |
| shiba      | Pixel 8      |
| husky      | Pixel 8 Pro  |
| akita      | Pixel 8a     |


```bash
lunch lineage_codename-userdebug
```

**Note:** After lunching, device dependencies, excluding vendor (proprietary-files/firmware), will be added to a local_manifest in .repo/local_manifests/roomservice.xml and synced via roomservice. Due to missing vendor, this will result in a failed lunch. The resolution for this will be handled in the next step.

## Generate vendor dependencies

Run the following command to download the appropriate factory image from Google and extract vendor (proprietary-files/firmware) automatically:

```bash
./lineage/scripts/pixel/device.sh codename
```

## Finally, lunch the device again and build

```bash
lunch lineage_codename-userdebug
m evolution
```
