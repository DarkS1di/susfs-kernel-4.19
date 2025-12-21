## Introduction ##
- An addon root hiding kernel patches and userspace module for KernelSU.

- The userspace tool `ksu_susfs`, as well as the ksu module, require a susfs patched kernel to work.

# Warning #
- This is only experimental code, that said it can harm your system or cause performance hit, **YOU ARE !! W A R N E D !!** already

## Compatibility ##
- The susfs kernel patches may differ for different kernel version or even on the same kernel version, you may need to create your own patches for your kernel.

## Patch Instruction (For GKI Kernel only and building from official google artifacts) ##
**- Prerequisite -**
1. All susfs patches are mainly based on the **original official KernelSU (the one from weishu)** with **tag / release tag**, so you should clone his repo with **tag / release tag** and clone this susfs branch with a **tag / release tag** or up to a commit message containing **"Bump version to vX.X.X"** to get a better patching result.
2. Since v2.0.0, SUSFS does not rely on kernel features like KPROBES, KRETPROBES and HAVE_SYSCALL_TRACEPOINTS, which means it will patch all the KernelSU code to use inline hooks now, even for the sucompat code.
3. SUSFS patches may conflict with some patches like custom manual hooks for sucompat since SUSFS already includes its own sucompat patches in KernelSU and kernel code.

**- Apply SUSFS patches -**
1. Clone the repo with a tag or release version, as they are more stable in general.
2. Run `cp ./kernel_patches/KernelSU/10_enable_susfs_for_ksu.patch $KERNEL_ROOT/KernelSU/`
3. Run `cp ./kernel_patches/50_add_susfs_in_kernel-<kernel_version>.patch $KERNEL_ROOT/`
4. Run `cp ./kernel_patches/fs/* $KERNEL_ROOT/fs/`
5. Run `cp ./kernel_patches/include/linux/* $KERNEL_ROOT/include/linux/`
6. Run `cd $KERNEL_ROOT/KernelSU` and then `patch -p1 < 10_enable_susfs_for_ksu.patch`
7. Run `cd $KERNEL_ROOT` and then `patch -p1 < 50_add_susfs_in_kernel.patch`, **if there are failed patches, you may try to patch them manually by yourself.**
8. If you want to make your kernel support other KSU manager variant, you can add its own hash size and hash in `ksu_is_manager_apk()` function in `KernelSU/kernel/apk_sign.c`
9. Make sure again to have `CONFIG_KSU` and `CONFIG_KSU_SUSFS` enabled before building the kernel, some other SUSFS feature may be disabled by default, you may enable/disable them via `menuconfig`, `kernel defconfig`, or change the `default [y|n]` option under each `config KSU_SUSFS_` option in `$KernelSU_repo/kernel/Kconfig` if you build with a new defconfig every time.

## Credits ##
susfs4ksu: https://gitlab.com/simonpunk/susfs4ksu/
