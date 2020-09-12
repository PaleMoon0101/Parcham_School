```console
root@moon:~/flag/forensic/challenge# file bamanbekhan
bamanbekhan: data
```
since 'file' command couldn't  tell us the file type and 'hexdump' doesn't work for this file, **binwalk** will provide more information about the file:

```console
root@moon:~/flag/forensic/challenge# binwalk
root@moon:~/flag/forensic/challenge# binwalk -e bamanbekhan
DECIMAL       HEXADECIMAL     DESCRIPTION
--------------------------------------------------------------------------------
40            0x28            Squashfs filesystem, little endian, version 4.0,
compression:gzip, size: 15633389 bytes, 2 inodes, blocksize: 131072 bytes, created: 2020-09-07 12:10:27
```

It seems that the first 40 Bytes of the file is useless! **dd** command help us to separate first 40 Byte of the file from other parts.
```console
root@moon:~/flag/forensic/challenge# dd if=bamanbekhan of=challenge.gz bs=1 skip=40
15634432+0 records in
15634432+0 records out
15634432 bytes (16 MB, 15 MiB) copied, 28.7094 s, 545 kB/s
root@moon:~/flag/forensic/challenge# ls
bamanbekhan  _bamanbekhan.extracted  challenge.gz
```
lets search the "_bamanbekhan.extracted", as you can see we have squashfs file system and in "squashfs-root" directory we can find *.img* file. It looks interesting!! lets mount
the disk image, we will have several *.img* files and one postscript file. first I open the postscript file:

yes, we found it :)))
```console
oot@moon:~/flag/forensic/challenge# cd _bamanbekhan.extracted
root@moon:~/flag/forensic/challenge/_bamanbekhan.extracted# ls
28.squashfs  squashfs-root
root@moon:~/flag/forensic/challenge/_bamanbekhan.extracted# cd squashfs-root
root@moon:~/flag/forensic/challenge/_bamanbekhan.extracted/squashfs-root# ls
zero.img
root@moon:~/flag/forensic/challenge/_bamanbekhan.extracted/squashfs-root# mkdir tmp
root@moon:~/flag/forensic/challenge/_bamanbekhan.extracted/squashfs-root# mount zero.img tmp/
root@moon:~/flag/forensic/challenge/_bamanbekhan.extracted/squashfs-root# cd tmp
root@moon:~/flag/forensic/challenge/_bamanbekhan.extracted/squashfs-root/tmp# file rand_
rand_: PostScript document text conforming DSC level 3.0, Level 2
```
the flag is: "parcham{I_l0v3_fOr3n5ics_to0}"
