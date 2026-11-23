# zszszszsz

Building OpenWrt for a specific router requires a matching device profile and explicit package choices. zszszszsz provides a build configuration for the TP-Link TL-WDR6500 v2 on the legacy `ar71xx/generic` target, with LuCI and selected network services.

**Check the hardware revision before building or flashing.** This configuration selects the v2 profile, not the v6 profile.

## Features

- Select the TL-WDR6500 v2 device profile and `mips_24kc` package architecture.
- Build a SquashFS root filesystem with size-oriented compiler options.
- Include LuCI with Simplified Chinese translations, firewall controls, scheduled reboot, and SFE controls.
- Include `dnsmasq-full`, PPPoE support, and Aliyun and DNSPod dynamic DNS scripts.
- Select V2Ray with its LuCI server interface, UPX compression, and bundled assets excluded.

## Installation

Use a Linux build host with Git, Make, a compiler toolchain, and the dependencies required by your chosen OpenWrt source revision. Build as an ordinary user, not root.

The configuration comes from a Lean's OpenWrt build environment. Obtain the source tree:

```sh
git clone https://github.com/coolsnowwolf/lede.git openwrt
cd openwrt
```

Check out a compatible source revision before proceeding. The supplied configuration selects Linux 4.9 and the legacy `ar71xx` target; the exact original source commit and feed revisions are not specified. A current checkout may no longer support these options or packages.

Once you have a compatible checkout, prepare its feeds and copy this project's `.config` into the source root. Replace `/path/to/zszszszsz` with your local project directory:

```sh
./scripts/feeds update -a
./scripts/feeds install -a
cp /path/to/zszszszsz/.config .config
cp .config .config.before-defconfig
make defconfig
diff -u .config.before-defconfig .config
```

Review the diff for removed device or package selections. A successful `make defconfig` does not prove that the source tree matches the configuration.

## Usage

zszszszsz is a configuration project, so it has no standalone executable or command-line arguments. Use the OpenWrt build commands from the prepared source directory:

```sh
make menuconfig
make download -j2
make -j2 V=s
```

In `menuconfig`, confirm the target, subtarget, and TL-WDR6500 v2 profile. Review package selections before saving. The configuration contains both target defaults and explicit package choices; a `CONFIG_DEFAULT_*` entry alone does not establish that a package is included.

Build outputs normally appear under `bin/targets/ar71xx/generic/`. Check the build log and generated image names before selecting an image. Small-flash devices may require fewer packages to fit the firmware partition.

**Treat this as a legacy build configuration, not a security-reviewed firmware release.** Package signature checking is disabled in the supplied configuration. Review that setting and the age of the kernel and packages before deployment. Back up the router and confirm its device-specific recovery procedure before flashing; no flashing command is provided here.

## License

MIT. This project is based on `zszszszsz/.config`. Retain the original MIT copyright and permission notices when redistributing its contents; see `LICENSE` for the full terms. OpenWrt and the packages built with this configuration retain their respective licenses.

