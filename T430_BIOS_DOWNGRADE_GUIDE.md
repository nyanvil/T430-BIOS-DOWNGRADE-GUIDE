# T430 BIOS DOWNGRADE GUIDE on linux

So you want to run 1vyrain but your bios is too new. The guides on youtube show ppl takin their motherboards off, what the fuck? IVPrep on github requires Windows, jesus fuckin christ. If you found my guide then you're in luck, motherfucker. Let's get right into this bullshit.

## ~ PREPARING YOUR BIOS ~

### BIOS (F1 DURING BOOT):

`SECURITY>UEFI BIOS UPDATE OPTION>SECURE ROLLBACK PREVENTION [DISABLED]`

whichever you prefer:

`STARTUP>UEFI/LEGACY BOOT PRIORITY:
LEGACY FIRST`

or

`STARTUP>UEFI/LEGACY BOOT:
LEGACY ONLY`

## ~ REQUIREMENTS: ~

- GPT-PARTITIONED USB DRIVE

check using:
```bash
sudo parted /dev/sda print | grep -i '^Partition Table'`
```
- IF IT SAYS `msdos` THEN U NEED TO FORMAT IT LIKE THIS:
```bash
sudo wipefs -a /dev/sdX
```
(REPLACE X WITH THE RIGHT DIGIT WHICH `lsblk` SHOWED)


- [2.64 iso downloaded](https://download.lenovo.com/ibmdl/pub/pc/pccbbs/mobiles/g1uj31us.iso)

- installed package: `genisoimage`

## ~ MOUNT THE ISO BEFORE WE MOD IT ~

```bash
geteltorito -o ./bios.img ~/Downloads/g1uj31us.iso
sudo mkdir -p /mnt/biosimg
sudo mount -t vfat ./bios.img /mnt/biosimg -o loop,offset=16384
```

## ~ LETS MOD ~

```bash
sudo nano /mnt/biosimg/AUTOEXEC.BAT
```

- NOW REPLACE `command.com` WITH:

```
C:\DRIVERS\FLASH\g1uj31us\WinFlash64s.exe /sd /file C:\DRIVERS\FLASH\g1uj31us\G1ETA4WW\$01D2000.FL1
```

## ~ GETTING READY TO FLASH ~

```bash
sudo umount /mnt/biosimg
```

- CHECK WHICH DEVICE IS YOUR USB USING `lsblk`

- WRITE THE MODDED BIOS TO IT

```bash
sudo dd if=./bios.img of=/dev/sdX bs=1M
```

(REPLACE X WITH THE RIGHT DIGIT)

- REBOOT YOUR LAPTOP
IT WILL REBOOT ITSELF ONCE MORE. DO NOT UNPLUG YOUR USB!

- AFTER A LONG BEEP THE SCREEN WILL GO DARK AND YOU MAY UNPLUG YOUR USB

- HIT F1 ONCE MORE TO ENSURE YOUR BIOS HAS UPDATED
