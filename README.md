# ubuntu-pxe
Install Ubuntu via PXE and recent kernel to workaround ACPI issues

Your new computer will boot from network (usually ethernet) using PXE and install Ubuntu from the internet.
You need to setup DHCP and TFTP to support PXE boot. And your installee will need internet access.

## Preparation

You need to get dhcp + tftp running. There are many guides out there, e.g.

  * <https://help.ubuntu.com/community/PXEInstallServer>
  * <https://help.ubuntu.com/community/Installation/QuickNetboot>

Enable IP forwarding / masquerading, e.g.:

  * `echo 1 > /proc/sys/net/ipv4/ip_forward`
  * `iptables -t nat -A POSTROUTING -o wlan0 -j MASQUERADE`

Copy `late_cmd` and `grub` to your tftp directory. Chdir to the tftp directory
and execute `download` to fetch the current netboot image from Ubuntu.

## Installation

Enable Network Boot in the BIOS, enable UEFI, but disable secure boot.
I.e. don't verify signatures on UEFI files.

Then boot from network. Install Ubuntu as usual.

Near the end of the installation, `preseed.txt` will download `late_cmd` through tftp and execute it.
This script in turn will download and install the most recent stable kernel from
<http://kernel.ubuntu.com/~kernel-ppa/mainline/>.
