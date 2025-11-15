# CMPE 283 – Assignment 2 : KVM Exit Statistics

## Contributions
Individual submission by **Mominuddin Mohammed**.

---

## Steps to Reproduce
1. Forked the Linux repo from [torvalds/linux](https://github.com/torvalds/linux) into my account [`mominuddinmohammed-lab/linux`](https://github.com/mominuddinmohammed-lab/linux).
2. Built kernel 6.18-rc5 on an outer VM (Ubuntu 22.04 with nested virtualization enabled).
3. Installed build dependencies (`build-essential`, `libncurses-dev`, `bison`, `flex`, `libssl-dev`, `libelf-dev`, `dwarves`, `dracut-core`, `git`, `qemu-kvm`).
4. Modified `arch/x86/kvm/vmx/vmx.c` to add exit counters and print statistics every 10 000 exits (omitting zero counts).
5. Rebuilt and installed the kernel; generated initramfs with `dracut`.
6. Booted into the custom kernel and verified via `uname -r`.
7. Launched an inner VM with `qemu-system-x86_64 -enable-kvm` and monitored `dmesg` for KVM Exit Stats output.

---

## Observations
- Exit counts increase roughly linearly with time.  
- Spikes occur during guest boot and I/O operations.  
- A full VM boot produces tens to hundreds of thousands of exits.  
- **Most frequent exits:** CPUID, MSR read/write, I/O instructions.  
- **Least frequent exits:** HLT, external interrupts.

---

## Example Output
=== KVM Exit Stats (VMX) after 10000 exits ===
Exit 30: 4123
Exit 48: 5877
=== KVM Exit Stats (VMX) after 20000 exits ===
Exit 30: 8300
Exit 48: 11700


---

## Repository Info
Branch: `cmpe283-exit-stats`  
Repo: [https://github.com/mominuddinmohammed-lab/linux](https://github.com/mominuddinmohammed-lab/linux)
