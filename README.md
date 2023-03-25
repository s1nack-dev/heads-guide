https://www.rototron.info/recover-bricked-bios-using-flashrom-on-a-raspberry-pi/

https://libreboot.org/docs/install/spi.html#reading

Download the top and bottom rom files from this commit https://app.circleci.com/pipelines/github/osresearch/heads/524/workflows/d13d6a02-1ffa-40ac-b721-e31ff69483d4/jobs/7458

grab the sha-sums from the Make Board check and compare them with what was downloaded

copy them over to  raspberry pi

Connect to the top chip and run `sudo flashrom -p linux_spi:dev=/dev/spidev0.0,spispeed=2000` and see if it detects the chip. You may get a list of possible chips, pick the one from the chart found here: https://osresearch.net/x230-maximized-flashing/

We're going to want to read the top one and then verified what was read is correct via
`sudo flashrom -p linux_spi:dev=/dev/spidev0.0,spispeed=2000 -r top.bin -c MX25L3206E/MX25L3208E`
and then
`sudo flashrom -p linux_spi:dev=/dev/spidev0.0,spispeed=2000 -v top.bin -c MX25L3206E/MX25L3208E`

Now we're going to write the new rom to the top chip via `sudo flashrom -p linux_spi:dev=/dev/spidev0.0,spispeed=2000 -c MX25L3206E/MX25L3208E -w heads-x230-hotp-maximized-v0.2.0-1400-g3ac896b-top.rom`

Now lets move onto the bottom chip

We're going to want to read the bottom one and then verified what was read is correct via
`sudo flashrom -p linux_spi:dev=/dev/spidev0.0,spispeed=2000 -r top.bin -c MX25L6406E/MX25L6408E`
and then
`sudo flashrom -p linux_spi:dev=/dev/spidev0.0,spispeed=2000 -v top.bin -c MX25L6406E/MX25L6408E`
Now we're going to write the new rom to the bottom chip via
`sudo flashrom -p linux_spi:dev=/dev/spidev0.0,spispeed=2000 -c MX25L6406E/MX25L6408E -w heads-x230-hotp-maximized-v0.2.0-1400-g3ac896b-bottom.rom`

Now remove the clip and power on the machine. If it doesn't boot the first time, try again.



https://forum.archive.openwrt.org/viewtopic.php?id=69483
truncate -s +12288K openwrt.bin


--

## Qubes install

download qubues iso
format a card to ext4(fat32 wont work due to 4gb file limit)

copy iso and iso.asc to usb stick manually

copy qubes master gpg key to seperate usb key

plug them all in.

upload quubes master gpg key to tpm

to boot usb into qubes installer

heads will verify the gpp key using the qubes master gpg key and 
the iso.asc signing key

install qubes usual. set up disk encryption


---

## setting up keys

after installing os, if you get errors or anything after heads boots, just pick an option that will get  you to the main menu

go to the recovery shell and set the clock to the "exact time down to the second" via `date -s 'yyyy-mm-dd hh:mm:ss' && hwclock -w`
This is mostly important if you want TOTP to be reliable.

Go to OEM Factory Reset/Re-Ownership

follow steps, save items to create pins and gpg key

Will wipe nitrokey(even when setting to the same admin/user pins)

make sure pings are no more than 12 characts.... could cause 
issues with nitro key

wait until everything is done. could take several minuites

when it's all done, when the system reboots, you may get a HOTP error, so "generate new TOTP/HOTP secret", and that should get everything
working again

---

Set up GPG key on Nitrokey https://docs.nitrokey.com/storage/linux/gpa


 copy public gpg key to seperate fat32 formatted usb stick 
Example filename: s1nack-nitrokey-032423.asc
--

