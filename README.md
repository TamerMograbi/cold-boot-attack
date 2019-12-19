# cold-boot-attack

show picture of frozen ram here

background section
the first paper about cold boot was released in 2009 and details why the ram is vulnerable
and how the attack is executed. the main idea is that RAM retains it's contents after several
secods after power is lost even at room temperature, thus cryptographic keys can be stolen and
laptop users are the ones who should be wary because most people keep their laptop
in sleep mode which keeps the RAM on. (hibernation stores ram content onto disk and so isn't vulnerable
to this attack)
Also the paper found that at -50 C (-58 F) less than 1% of the bits decayed after 10 minutes without power!

Attack overview
I installed a lightweight OS on a usb and complied Lime on this lightweight OS.
afterwards i plugged in the usb to the victim laptop which was in sleep mode,
sprayed on the ram with a can of pressurized air (while holding it upside down) to cool it
and afterwards i shut down the laptop and quickly turned it back on and booted via usb
into my OS, i then used lime to extract the ram content onto disk.

write ram to disk section
I first needed to find a way to write ram onto disk, in some cases you only need cat /disk/mem
or something similar but in newer linux versions this doesn't work. I looked online and found numerous ways
to do this, i ended up choosing Lime https://github.com/504ensicsLabs/LiME.

Lime section
lime is a linux kernel module, when you install the module using Sudo insmod ./lime.ko “path=../linux.mem format=raw”
it dumps ram content onto ../linux.mem. One bad thing about lime is that if you
compiled it on a machine with kernel version x then it won't work on other machines
that use any other version.

I had to go through a lot of operating systems in order to find one that works well and is lightweight.
I tried Lubuntu,linux lite,puppy linux, DSL, Bhodi linux, Archlinux, slax, salix.
I ended up successfully executing the attack on peppermint OS.

I tried this attack on an old sony 2012 laptop and an old 2012 macbook air and it worked on
both of them. just to see that it's working correctly, I flooded the ram with the string batmannanana
using the small python program add_str.py. i kept it running for a few minutes, i cooled the ram using the pressurized air can and then i shut the laptop down,
booted into peppermint OS, immediately dumped ram onto disk and ran the command string <ram dump file> | grep batman.
If i saw a lot of copies of the word then the attack was successful. (you will see a lot of words such as ba@man 1atman batpan
and many other variations as not all bits in ram will hold their values all the time.)
