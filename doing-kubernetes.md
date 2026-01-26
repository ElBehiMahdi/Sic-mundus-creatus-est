- 192.168.100.230
<interface type="network">
  <mac address="52:54:00:12:e8:53"/>
  <source network="network" portid="e7dc4083-5335-4a72-9aff-45d21c434ad8" bridge="virbr0"/>
  <target dev="vnet1"/>
  <model type="virtio"/>
  <alias name="net0"/>
  <address type="pci" domain="0x0000" bus="0x01" slot="0x00" function="0x0"/>
</interface>

- https://github.com/SpaceAceMonkey/spaceace-arch-kubernetes-unraid?tab=readme-ov-file#step-2-provision-virtual-machines
- https://chat.deepseek.com/a/chat/s/dfe4175a-3288-4e61-8ae4-e2ef1703a985
- follow step 3 https://github.com/SpaceAceMonkey/spaceace-arch-kubernetes-unraid?tab=readme-ov-file#step-3-install-arch-linux
- commands to review :
  - # If you're in the chroot environment already:
  nano /etc/vconsole.conf
  - Complete BIOS GRUB installation would be
  arch-chroot /mnt
  mkdir /boot/efi
  mount /dev/vda1 /boot/efi
  grub-install --target=i386-pc /dev/vda   
  grub-mkconfig -o /boot/grub/grub.cfg


