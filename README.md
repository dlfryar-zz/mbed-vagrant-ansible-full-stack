=== Step #

Install Vagrant https://vagrantup.com

=== Step #

Install VirtualBox https://www.virtualbox.org/

=== Step #

Clone this repo
    git clone https://github.com/ARMmbed/blah

=== Step #

Edit vagrant.env and add your ARM mbed Cloud credentials

=== Step #

Copy identity_dev_security.c which you obtained from
https://portal.mbedcloud.com/Developer tools/Certificate
to this directory

=== Step #

Start up the environment
   vagrant up

=== Step #

Connect a FRDM-K64F https://developer.mbed.org/platforms/FRDM-K64F/
board to your USB port

=== Step #

Connect the Ethernet port on the FRDM-K64F to your switch or router.

If you are behind a NAT'ed device then you must open the CoAP ports
on your router in order for this to work.

Virtual Servers / Port Forwarding
|-------------|--------------|------|--------------------|------------|
| Description | Inbound Port | Type | Private IP Address | Local Port |
|-------------|--------------|------|--------------------|------------|
|  FRDM-K64F  |  5683-5684   | Both |    192.168.0.18    | 5683-5684  |
|-------------|--------------|------|--------------------|------------|

=== Step #

Copy the firmware binary that was built to the FRDM-K64F board in order
to flash the image.
    cp Source/mbed-os-example-client/BUILD/blah /mnt/MBED

=== Step #

Connect to the serial port on the FRDM-K64F to reset after flashing and
to watch connection details.

    sudo minicom -D /dev/ttyACM0 -b 115200

Type "ctrl + a" let go then "f" to send a break to the board once the
flashing activity led stops blinking rapidly from the copy/flashing step
above.

Now watch for connection messages once the board comes back up

=== Step #

Point your browser to http://localhost:8000

This is a NodeJS application server running on a virtual machine on
your computer.  It's the one called server if you were to list the VM's
created by typing "vagrant status"

This NodeJS server is running against your mbed Cloud instance interacting
with the REST API.

MyComputer --> ServerVM --> REST API --> mbed Cloud

The data and interaction contained in the NodeJS application is interacting
with the FRDM-K64F.  If everything connected correctly and your credentials
are valid you should be able to see active data and make changes on the
FRDM-K64F from the browser.

*note: since we don't have a publically routable IP address, the NodeJS
       application implements polling to make this demo work everywhere
       if you had a public IP then webhook notifications are the quickest
       and lightest weight choice

FRDM-K4F --> CoAP --> mbed Cloud

=== Step #

When you're all done tear the sucker down
    vagrant destroy -f
