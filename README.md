# CMPE 283 – Assignment 2: KVM Exit Statistics

## Contributions
Individual submission.

## Steps to Reproduce
1. Fork torvalds/linux to `mominuddinmohammed-lab/linux`.
2. On the outer VM (GCE), install deps:
   `sudo apt-get install -y build-essential libncurses-dev bison flex libssl-dev libelf-dev dwarves dracut-core qemu-kvm git`
3. Clone and build kernel (6.18-rc5), ensure NVMe/virtio/ext4 as built-in; create initramfs with dracut.
4. Add exit counters in `arch/x86/kvm/vmx/vmx.c`:
   - Increment per-type and total.
   - `printk` one line per non-zero exit type every 10,000 total exits.
5. Rebuild, install, one-time boot via `grub-reboot`.
6. Launch inner VM with `-enable-kvm` and observe `dmesg` on the host.
7. Commit and push changes to branch `cmpe283-exit-stats`.

## Observations
- Exits grow roughly linearly during idle/steady workloads; spikes during boot and I/O bursts.
- A full VM boot typically triggers on the order of tens to hundreds of thousands of exits (depends on image and device init).
- Most frequent: CPUID, MSR reads/writes, I/O-related exits.
- Least frequent: HLT (when guest idles with PV timers), rare faults/edge exits.

