---
title: "Parse eMMC Extended CSD (ECSD) Registers with Python"
excerpt: "Decode the 512 byte mess of the card status registers with a quick Python script"
category: linux
tags: [linux, hardware, emmc, embedded, python]
header:
  image: https://i.imgur.com/46C7eL7.jpg
  overlay_color: "#000"
  overlay_filter: "0.5"
  overlay_image: https://i.imgur.com/46C7eL7.jpg
---

## eMMC and Extended CSD Registers

The other day I needed to read of the data from the Extended CSD (aka ECSD) registers of a eMMC chip I was having a disagreement with.  Linux makes this kind of easy with a kernal that has debugfs support enabled:

    $ mount -t debugfs debugfs /d
    $ cat /d/mmc0/mmc0:0001/ext_csd
	0000000000000001030100000000000000000000000000000000000000000000000000000000
    0000000000000000000000000000000000000000000000000000000000000000000000000000
    0000000000000000000000000000000000000000000000000000000000000000000000000000
    0000000000000000000000000000000000000000000000000000000000000000000000000000
    0000000000000000000000000000000000000000000000000000000000000000000000000000
	0000000000000000000000000000000000000000000000000000000000000000000000000000
    0000000000000000000000000000000000000000000000000000000000000000000000000002
    000000087a000000000000000006150203070010060801010108080010000072800000080808
    0808080000000000010200070002000500000000000001000200000000000000000000000000
    000100050000000000030001ca00000000000000000000000000000000000000000000000000
	0000000000000000000000000000000000000000000000000000000000000000000000000000
	0000000000000000000000000000000000000000000000000000000000000000000000000000
    0000000000000000000000000000000000000000000000000000000000000000000000000000
	000000000000000000000000000000000000

Making sense of the 512 bytes string is another issue all together.

## Parse Extended CSD Data with Python

	$ ./parse-ext-csd.py
	0:      00 00 00 00   00 00 00 00   00 00 00 00   00 00 00 00
	16:     00 00 00 00   00 00 00 00   00 00 00 00   00 00 00 00
	32:     00 00 00 00   00 00 00 00   00 00 00 00   00 00 00 00
	48:     00 00 00 00   00 00 00 00   00 00 00 00   00 00 00 00
	64:     00 00 00 00   00 00 00 00   00 00 00 00   00 00 00 00
	80:     00 00 00 00   00 00 00 00   00 00 00 00   00 00 00 00
	96:     00 00 00 00   00 00 00 00   00 00 00 00   00 00 00 00
	112:    00 00 00 00   00 00 00 00   00 00 00 00   00 00 00 00
	128:    00 00 00 00   00 00 00 00   00 00 00 00   00 00 00 00
	144:    00 00 00 00   00 00 00 00   00 00 00 00   00 ca 01 00
	160:    03 00 00 00   00 00 05 00   01 00 00 00   00 00 00 00
	176:    00 00 00 00   00 00 00 02   00 01 00 00   00 00 00 00
	192:    05 00 02 00   07 00 02 01   00 00 00 00   00 08 08 08
	208:    08 08 08 00   00 80 72 00   00 10 00 08   08 01 01 01
	224:    08 06 10 00   07 03 02 15   06 00 00 00   00 00 00 00
	240:    00 7a 08 00   00 00 02 00   00 00 00 00   00 00 00 00
	256:    00 00 00 00   00 00 00 00   00 00 00 00   00 00 00 00
	272:    00 00 00 00   00 00 00 00   00 00 00 00   00 00 00 00
	288:    00 00 00 00   00 00 00 00   00 00 00 00   00 00 00 00
	304:    00 00 00 00   00 00 00 00   00 00 00 00   00 00 00 00
	320:    00 00 00 00   00 00 00 00   00 00 00 00   00 00 00 00
	336:    00 00 00 00   00 00 00 00   00 00 00 00   00 00 00 00
	352:    00 00 00 00   00 00 00 00   00 00 00 00   00 00 00 00
	368:    00 00 00 00   00 00 00 00   00 00 00 00   00 00 00 00
	384:    00 00 00 00   00 00 00 00   00 00 00 00   00 00 00 00
	400:    00 00 00 00   00 00 00 00   00 00 00 00   00 00 00 00
	416:    00 00 00 00   00 00 00 00   00 00 00 00   00 00 00 00
	432:    00 00 00 00   00 00 00 00   00 00 00 00   00 00 00 00
	448:    00 00 00 00   00 00 00 00   00 00 00 00   00 00 00 00
	464:    00 00 00 00   00 00 00 00   00 00 00 00   00 00 00 00
	480:    00 00 00 00   00 00 00 00   00 00 00 00   00 00 00 00
	496:    00 00 00 00   00 00 01 03   01 00 00 00   00 00 00 00
	SEC_BAD_BLK_MGMNT[134] = 0x0
	FW_CONFIG[169] = 0x0
	ERASE_GROUP_DEF[175] = 0x0
	EXT_CSD_REV[192] = 0x5
	CSD_STRUCTURE[194] = 0x2
	CARD_TYPE[196] = 0x7
	ERASE_TIMEOUT_MULT[223] = 0x1
	HC_ERASE_GRP_SIZE[224] = 0x8
	BOOT_INFO[228] = 0x7
	SEC_TRIM_MULT[229] = 0x3
	SEC_ERASE_MULT[230] = 0x2
	SEC_FEATURE_SUPPORT[231] = 0x15

## Python Code Snippet

Copy your `ext_csd` file contents to the `ecsd_str` of this script.  Someday I might spend 5 minutes to make it read `stdin`, until then you have to manually copy it.

    #!/usr/bin/env python
    """
    Author: Kyle Manna <kyle@kylemanna.com>
    Blog: https://blog.kylemanna.com
     cat /d/mmc0/mmc0:0001/ext_csd
     0000000000000001030100000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000002000000087a0000000000000000061502030700100608010101080800100000728000000808080808080000000000010200070002000500000000000001000200000000000000000000000000000100050000000000030001ca00000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000
     """
    import binascii
    import re
    import sys

    def str2bytearray(s):
        if len(s) % 2:
            s = '0' + s

        reorder = True
        if reorder:
            r = []
            i = 1
            while i <= len(s):
                r.append(s[len(s) - i - 1])
                r.append(s[len(s) - i])
                i += 2
            s = ''.join(r)

        out = binascii.unhexlify(s)

        return out


    if __name__ == '__main__':

        ecsd_str = '0000000000000001030100000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000002000000087a0000000000000000061502030700100608010101080800100000728000000808080808080000000000010200070002000500000000000001000200000000000000000000000000000100050000000000030001ca00000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000'
        #ecsd_str = '320100'
        ecsd = str2bytearray(ecsd_str)

        line_len = 16
        i = 0
        while i < len(ecsd):
            sys.stdout.write("{0:04x}:\t".format(i))
            for j in range(line_len):
                if (i < len(ecsd)):
                    sys.stdout.write("{0:=02x}".format(ord(ecsd[i])))
                    i = i + 1
                else:
                    break

                if (j == (line_len - 1)): pass
                elif (i % 4): sys.stdout.write(" ")
                else: sys.stdout.write("   ")

            sys.stdout.write("\n")

        print "SLC_DEVICE_HEALTH_STATUS[87] = 0x{:x}".format(ord(ecsd[87]))
        print "MLC_DEVICE_HEALTH_STATUS[94] = 0x{:x}".format(ord(ecsd[94]))
        print "SEC_BAD_BLK_MGMNT[134] = 0x{:x}".format(ord(ecsd[134]))
        print "FW_CONFIG[169] = 0x{:x}".format(ord(ecsd[169]))
        print "ERASE_GROUP_DEF[175] = 0x{:x}".format(ord(ecsd[175]))
        print "EXT_CSD_REV[192] = 0x{:x}".format(ord(ecsd[192]))
        print "CSD_STRUCTURE[194] = 0x{:x}".format(ord(ecsd[194]))
        print "CARD_TYPE[196] = 0x{:x}".format(ord(ecsd[196]))
        print "ERASE_TIMEOUT_MULT[223] = 0x{:x}".format(ord(ecsd[223]))
        print "HC_ERASE_GRP_SIZE[224] = 0x{:x}".format(ord(ecsd[224]))
        print "BOOT_INFO[228] = 0x{:x}".format(ord(ecsd[228]))
        print "SEC_TRIM_MULT[229] = 0x{:x}".format(ord(ecsd[229]))
        print "SEC_ERASE_MULT[230] = 0x{:x}".format(ord(ecsd[230]))
        print "SEC_FEATURE_SUPPORT[231] = 0x{:x}".format(ord(ecsd[231]))


The code also lives in a [Github Gist](https://gist.github.com/kylemanna/5692543).