# First thanks to [Zitaotech](https://github.com/ZitaoTech)

I modify his fireware to our project,whitout his help ,we cannt see trackpad on uconsole ,thanks to him and his many interesting productions.

# uconsole BB9900 wireless/usb Keyboard: zmk-config

------------------------------

The key:

Part ONE--USB

1.when connect uconsole via usb,ble dont work!

Part TWO--BLE

1.RFN+1 2 3 is three different equipment ,RFN+ESC is Clean BLE(when you wanna connect new equipment and clean you ble info)

2.RFN+ (\\|) is bootloader

3.RFN+LFN is soft-reset


Part Three New update need you help

1.LFN +trackpad is ↑↓←→

2.Caps light！！

3.outside Crystal oscillator works！

--------------------------------
Hey 👋 welcome. Use this repo to generate your own ZMK keymap for the BB9900 BLE keyboard.
[Keycode that you can use in ZMK firmware](https://zmk.dev/docs/codes)
[Different behaviors that you can use in ZMK firmware](https://zmk.dev/docs/behaviors)
## Get started
0. Register a github account if you don't have one.
1. Fork this repo.![fork](https://github.com/ZitaoTech/zmk-config_9900/assets/145678024/4ffc71b9-0ed3-4ae9-ace7-99078dd1d9bc)
2. Open up `config/bb9900.keymap` and edit the keymap to your liking.![image](https://github.com/ZitaoTech/zmk-config_9900/assets/145678024/a0900a5c-6650-4794-9d11-a17c380a973d)
3. After editing the keymap, choose commit changes![image](https://github.com/ZitaoTech/zmk-config_9900/assets/145678024/c708dbd0-6c90-49da-aeda-053668ae43c8)
 and then check the Github Actions section.![image](https://github.com/ZitaoTech/zmk-config_9900/assets/145678024/fb534054-add6-4517-8643-8270cbf6d8c7)
 Your new firmware file should be available for download.![image](https://github.com/ZitaoTech/zmk-config_9900/assets/145678024/ae6a1646-c8ab-4966-b969-12e68ecaa0ab)
![image](https://github.com/ZitaoTech/zmk-config_9900/assets/145678024/a6140108-9e27-4d51-aa42-ba12233b8738)
5. Unzip the firmware.zip file. You should see one files: `bb9900-zmk.uf2`.
6. Flash the keyboard with your new firmware.

## Flashing the firmware

Legacy guide/generic guide: [How to flash the firmware](https://github.com/ZitaoTech/BB9900-USB_BLE_Keyboard?tab=readme-ov-file#-how-to-update-the-firmware---)

### Uconsole Trackpad keyboard guide
For hack2you.tech's V2.0 PCB, you can flash using ssh and uconsole.

_0. Prerequisites:_
- A computer with ssh client/a phone with ssh client app (e.g. Termius)
- Your uconsole connected to the same network as your computer/phone.

_1. Get the uconsole's IP address:_
1. On your uconsole, open the terminal and type `ip addr show wlan0`. Look for the line that starts with `inet` and note down the IP address (it should look something like `192.168.x.x`).

_2. Copy the firmware to the uconsole:_
On the uconsole:
1. Download the latest firmware from the Github Actions section of this repo.
2. Unzip the downloaded firmware.zip file. You should see one file: `bb9900-zmk.uf2`.
3. Copy it to a path you remember (`~/`, or `/home/[your-username]`) are good choices.

Or, from your other computer:
1. Open your terminal
2. Use the `scp` command to copy the firmware file to your uconsole. The command should look like this:
```bash
scp /path/to/bb9900-zmk.uf2 [your-username]@[uconsole-ip-address]:~/
```

_3. Put your keyboard into bootloader mode:_
1. On your uconsole, hold the Right Fn key and the `\|` key at the same time.

This will put your keyboard into bootloader mode (you'll know cause you can't use it rn).

_4. SSH into your uconsole:_
1. Open your terminal and use the `ssh` command to connect to your uconsole.
```bash
ssh [your-username]@[uconsole-ip-address]
```
2. Enter your password when prompted.
3. You should now be logged into your uconsole via ssh.

_5. Find the UF2 drive:_
1. In the SSH terminal
```bash
ls /media/[your-username]/
```
2. You should see a drive that wasn't there before, or more than one drive (assuming your boot drive is there).
- Mine was called `ADM840BOOT1`
3. cd to that drive and confirm contents
```bash
cd /media/[your-username]/[UF2-drive-name]/
ls
```
4. You should see a file called `INFO_UF2.TXT`
```
  /media/james/ADM840BOOT1                                                                                                 took  0.0385s 🏠 Local with james@uconsole
❯ ls
CURRENT.UF2  INDEX.HTM  INFO_UF2.TXT
  /media/james/ADM840BOOT1                                                                                                 took  0.0292s 🏠 Local with james@uconsole
❯ cat INFO_UF2.TXT
UF2 Bootloader 0.10.0
Model: AtelierDuMaker NRF52840 Breakout
Board-ID: ADM_B_NRF52840_1
Date: Feb  3 2026
SoftDevice: not found
```

_6. Flash the firmware:_
1. Now, copy the firmware file to the UF2 drive. You can do this with the `cp` command:
```bash
cp ~/bb9900-zmk.uf2 /media/[your-username]/[UF2-drive-name]/
```

The ssh session will "hang" for a few seconds. When it's done flashing it will give you your next prompt.
The keyboard automatically restarts after flashing, so you should be able to use it right away.

