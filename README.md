# This is a fix for no backlight control on Acer Switch 10 SW5-012 (Currently tested on bios 1.20)

All credit to @jwrdegoede, this is just a tutorial how to patch and compile dsdt overlay

## Install dependencies
`apt install acpica-tools`

## Get DSDT table
`cat /sys/firmware/acpi/tables/DSDT > dsdt.dat`

## Disassemble
`iasl -d dsdt.dat`

## Patch the table with the diff file
`patch dsdt.dsl < diff`

## Compile
`iasl -tc dsdt.dsl`

## Create file structure for the cpio archive
`mkdir -p kernel/firmware/acpi`

## Copy the overlay
`cp dsdt.aml kernel/firmware/acpi`

## Create the cpio archive
`find kernel | cpio -H newc --create > acpi_override`

## Copy the overlay archive to the /boot directory
`cp acpi_override /boot`

## Edit /etc/default/grub and add:
`GRUB_EARLY_INITRD_LINUX_CUSTOM="acpi_override"`

This will add the overlay before your initrd

## Remake grub config
`update-grub`

## Reboot and try
