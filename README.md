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