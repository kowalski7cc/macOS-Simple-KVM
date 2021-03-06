# macOS-Simple-KVM

Documentation to set up a simple macOS VM in QEMU, accelerated by KVM.

By [@FoxletFox](https://twitter.com/foxletfox), and the help of many others. Find this useful? You can donate [on Coinbase](https://commerce.coinbase.com/checkout/96dc5777-0abf-437d-a9b5-a78ae2c4c227) or [Paypal!](https://www.paypal.com/cgi-bin/webscr?cmd=_donations&business=QFXXKKAB2B9MA&item_name=macOS-Simple-KVM).

New to macOS and KVM? Check [the FAQs.](docs/FAQs.md)

## Getting Started

You'll need a Linux system with `qemu` (3.1 or later), `python3`, `pip` and the KVM modules enabled. A Mac is **not** required. Some examples for different distributions:

```bash
sudo apt-get install qemu-system qemu-utils python3 python3-pip  # for Ubuntu, Debian, Mint, and PopOS.
sudo pacman -S qemu python python-pip            # for Arch.
sudo xbps-install -Su qemu python3 python3-pip   # for Void Linux.
sudo zypper in qemu-tools qemu-kvm qemu-x86 qemu-audio-pa python3-pip  # for openSUSE Tumbleweed
sudo dnf install qemu qemu-img python3 python3-pip # for Fedora
sudo emerge -a qemu python:3.4 pip # for Gentoo
```

## Step 1

Run `jumpstart.sh` to download installation media for macOS (internet required). The default installation uses Catalina, but you can choose which version to get by adding either `--high-sierra`, `--mojave`, or `--catalina`. For example:

```bash
./jumpstart.sh --mojave
```
> Note: You can skip this if you already have `BaseSystem.img` downloaded. If you have `BaseSystem.dmg`, you will need to convert it to a raw image, e.g. `qemu-img convert -O raw BaseSystem.dmg BaseSystem.img`.

## Step 2a

Create an empty hard disk using `qemu-img`, changing the name and size to preference:

```bash
qemu-img create -f qcow2 MyDisk.qcow2 64G
```
> Note: If you use a name other than MyDisk.qcow2, you will need to update `basic.sh` (e.g. `sed -i 's/MyDisk.qcow2/Master.qcow2/' basic.sh`)

> Note: If you're running on a headless system (such as on Cloud providers), you will need `-nographic` and `-vnc :0 -k en-us` for VNC support.

> **Note**: If you're running on a headless system (such as on Cloud providers), you will need `-nographic` and `-vnc :0 -k en-us` for VNC support.

Then run `basic.sh` to start the machine and install macOS. Remember to partition in Disk Utility first!

## Step 2b (GNOME Boxes and Virtual Machine Manager)

If instead of QEMU, you'd like to import the setup into GNOME Boxes or Virt-Manager for convenience or for further configuration, just run `./make.sh --install` or the shorter `./make.sh -i`.

No root is needed because images will be installed in your home, with any other your Boxes virtual machine.

## Step 2c (Headless Systems)

If you're using a cloud-based/headless system, you can use `headless.sh` to set up a quick VNC instance. Settings are defined through variables as seen in the following example. VNC will start on port `5900` by default.

```bash
HEADLESS=1 MEM=1G CPUS=2 SYSTEM_DISK=MyDisk.qcow2 ./headless.sh
```

## Step 3

**You're done!**

To fine-tune the system and improve performance, look in the `docs` folder for more information on [adding memory](docs/guide-performance.md), setting up [bridged networking](docs/guide-networking.md), adding [passthrough hardware (for GPUs)](docs/guide-passthrough.md), tweaking [screen resolution](docs/guide-screen-resolution.md), and enabling sound features.
