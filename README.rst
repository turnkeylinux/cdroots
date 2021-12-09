gfxboot turnkey notes
=====================

- gfxboot-turnkey/isolinux

    - bootlogo      # packed archive of src/gfxboot-turnkey-bootlogo

- src/gfxboot-turnkey-bootlogo

    - en.tr         # tweaked and compiled gfxboot-theme-ubuntu
    - bootlogo      # tweaked and compiled gfxboot-theme-ubuntu

    gfxboot --archive gfxboot-turnkey-bootlogo --pack-archive bootlogo

- gfxboot-theme-ubuntu

    apt-get install gfxboot gfxboot-dev make xz-utils wget
    wget ..../gfxboot-theme-ubuntu...., unpack

    cd gfxboot-theme-ubuntu
    rm po/*.po

    - tweak panel.inc           # see below
    - tweak dia_fulloptions.inc # see below

    make
    copy out install/{bootlogo, en.tr}


diff panel.inc.orig panel.inc
-----------------------------

39d38
<       [ .panel.both [ keyF1 0 "F1" /panel.help /panel.help.width /panel.help.update .undef ] ]
43,47d41
<
<       [ .panel.both [ keyF2 0 "F2" /panel.lang /panel.lang.width /panel.lang.update /lang.init ] ]
<       [ .panel.both [ keyF3 0 "F3" /panel.keymap /panel.keymap.width /panel.keymap.update /keymap.init ] ]
<       [ .panel.both [ keyF4 0 "F4" /panel.modes /panel.modes.width /panel.modes.update /modes.init ] ]
<       [ .panel.both [ keyF5 0 "F5" /panel.access /panel.access.width /panel.access.update /access.init ] ]


diff dia_fulloptions.inc.orig dia_fulloptions.inc
-------------------------------------------------

51,54c51
<     is_live not { /txt_expert_mode } if
<     /txt_acpi_off /txt_noapic /txt_nolapic /txt_edd_on /txt_nodmraid
<     /txt_nomodeset
<     /txt_option_free
---
>     /txt_acpi_off /txt_noapic /txt_nolapic
57c54
<   xmenu .xm_title /txt_other_options put
---
>   xmenu .xm_title /txt_bootoptions put


Generate gfxboot-turnkey/efi.img and other files
================================================

Install deps::

   apt update
   apt install -y grub-efi-amd64-bin grub-imageboot grub-pc-bin grub-pc

Generate grubrescue disk and extract components::

   grub-mkrescue -o /tmp/grub.iso
   mkdir /tmp/grubcd
   mount -o loop /tmp/grub.iso /tmp/grubcd
   cp /tmp/grubcd/efi.img gfxboot-turnkey/
   cp -R /tmp/grubcd/boot gfxboot-turnkey/

