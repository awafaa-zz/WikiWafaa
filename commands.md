#Useful Commands#
These commands are for Linux (openSUSE), unless specified.

##Output command to file##

`<command> >> <filelocation>/<filename> 2>&1`

##Mount an ISO image##

`mount /<path>/<file>.iso /mnt/ISO -t iso9660 -o ro,loop=/dev/loop0`

##Video capture using dvgrab##

`dvgrab -stin -timestamp -f mpeg2 $FILENAME`

##Vacuming an sqlite database##
`for f in ~/.mozilla/firefox/*/*.sqlite; do sqlite3 $f 'VACUUM;'; done`

##Creating a simple patch##

`diff -Nadru old/ new/ > file.diff`

##Defuzzing Patches##

`quilt setup package.spec`
`cd package-ver`
`while quilt push; do quilt refresh; done`

##Finding out how much space directories are taking up##

`du -k | sort -n | perl -ne 'if ( /^(\d+)\s+(.*$)/){$l=log($1+.1);$m=int($l/log(1024)); printf ("%6.1f\t%s\t%25s %s\n",($1/(2**(10*$m))),(("K","M","G","T","P")[$m]),"*"x (1.5*$l),$2);}'`

##When grub is misconfigured##

`root (hd0,1)`
`kernel /boot/vmlinuz`
`initrd /boot/initrd`
`boot`

Once booted you can edit `/boot/grub/menu.lst` and change the names of kernel and initrd to just `vmlinuz` and `initrd`

To fix initrd do the following:

`mount /dev/sdaX /mnt && mount --bind /proc /mnt/proc && mount --bind /sys /mnt/sys && mount --bind /dev /mnt/dev && chroot /mnt`

Then:
`mkinitrd -k /boot/vmlinuz-blabla -i /boot/initrd-blabla`
