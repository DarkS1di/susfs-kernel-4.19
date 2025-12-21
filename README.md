## Introduction ##
- An addon root hiding kernel patches and userspace module for KernelSU.

- The userspace tool `ksu_susfs`, as well as the ksu module, require a susfs patched kernel to work.

# Warning #
- This is only experimental code, that said it can harm your system or cause performance hit, **YOU ARE !! W A R N E D !!** already

## Compatibility ##
- The susfs kernel patches may differ for different kernel version or even on the same kernel version, you may need to create your own patches for your kernel.

## Patch Instruction ##
**- Prerequisite -**
1. All susfs patches are mainly based on the **original official KernelSU (the one from weishu)** with **tag / release tag**, so you should clone his repo with **tag / release tag** and clone this susfs branch with a **tag / release tag** or up to a commit message containing **"Bump version to vX.X.X"** to get a better patching result.
2. Since v2.0.0, SUSFS does not rely on kernel features like KPROBES, KRETPROBES and HAVE_SYSCALL_TRACEPOINTS, which means it will patch all the KernelSU code to use inline hooks now, even for the sucompat code.
3. SUSFS patches may conflict with some patches like custom manual hooks for sucompat since SUSFS already includes its own sucompat patches in KernelSU and kernel code.

**- Apply SUSFS patches -**
1. Clone the repo with a tag or release version, as they are more stable in general.
2. Run `cp ./kernel_patches/50_add_susfs_in_kernel-<kernel_version>.patch $KERNEL_ROOT/`
3. Run `cp ./kernel_patches/fs/* $KERNEL_ROOT/fs/`
4. Run `cp ./kernel_patches/include/linux/* $KERNEL_ROOT/include/linux/`
5. Run `cd $KERNEL_ROOT` and then `patch -p1 < 50_add_susfs_in_kernel.patch`, **if there are failed patches, you may try to patch them manually by yourself.**

## Credits ##
susfs4ksu: https://gitlab.com/simonpunk/susfs4ksu/
