# Linux Kernel

The armbian's developers have done a lot of work to make the kernel work on RK3588/RK3588S based boards, and This Flake is based on their work.

Related Armbian projects:

- <https://github.com/armbian/build>
- <https://github.com/armbian/linux-rockchip>


test command:

```bash
./compile.sh build BOARD=orangepi5 BRANCH=legacy BUILD_DESKTOP=no BUILD_MINIMAL=yes KERNEL_CONFIGURE=yes RELEASE=jammy
```

Other informations:

- the SBC specific build configuration:
  - <https://github.com/armbian/build/blob/main/config/boards/orangepi5.conf>
  - <https://github.com/armbian/build/blob/main/config/boards/orangepi5-plus.conf>
  - <https://github.com/armbian/build/blob/main/config/boards/rock-5a.wip>
  - useful options defined in the above files:
    - BOARDFAMILY: choose the right board family, which will be used below.
    - BOOT_FDT_FILE: the dtb file name.
    - for some boards, the `BOOTSOURCE` is overridden to a different repo.
      - e.g. orangepi5/orangepi5plus: the `BOOTSOURCE` is overridden to https://github.com/orangepi-xunlong/u-boot-orangepi.git
- Board Families
  - `BOARDFAMILY="rockchip-rk3588"`:
    - source: https://github.com/armbian/build/blob/main/config/sources/families/rockchip-rk3588.conf
    - uboot: `BOOTSOURCE='https://github.com/radxa/u-boot.git'`
    - BRANCH:
      - legacy: the default option, kernel 5.10, maintained by armbian.
        - `KERNELPATCHDIR='rk35xx-legacy'`
          - all these patches has already been merged into the mainline kernel: https://github.com/armbian/build/commit/afa8775f3e0218b039185b34831b0feee7fe5a50
        - `LINUXFAMILY=rk35xx`
      - collabora: kernel 6.4, maintained by Collabora.
        - `LINUXCONFIG='linux-rockchip-rk3588-'$BRANCH`
- the kernel config:
  - default to `LINUXCONFIG="linux-${LINUXFAMILY}-${BRANCH}"` + `.config`
    - defined at:
      - https://github.com/armbian/build/blob/main/lib/functions/configuration/main-config.sh#L291
      - https://github.com/armbian/build/blob/main/lib/functions/compilation/kernel-config.sh#L14-L22
  - so the default kernel config for orangepi5 / orangepi5plus / rock-5a is:
    - BRANCH=legacy: <https://github.com/armbian/build/blob/main/config/kernel/linux-rk35xx-legacy.config>
    - BRANCH=collabora: <https://github.com/armbian/build/blob/main/config/kernel/linux-rockchip-rk3588-collabora.config>
- the initial commit for orangepi5 in armbian/build:
  - <https://github.com/armbian/build/commit/18198b1d7d72cbef44228e7cb44a078cb8e03f27#diff-999cd9038268b0c128f7342a957b87fa0b4b12536e4cd2abd54c9a17180188e1>

The SBC's kernel config in this directory is based on the armbian's kernel config listed above.

## References

- [RK3588 Mainline Kernel support - Rockchip RK3588 upstream enablement efforts](https://gitlab.collabora.com/hardware-enablement/rockchip-3588/notes-for-rockchip-3588/-/blob/main/mainline-status.md)
