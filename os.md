---
charset: UTF-8
layout: default
---

# Troubles with Linux, Unix, Posix, etc #

* Non-blocking IO does exist for ordinary files
    * <http://www.remlab.net/op/nonblock>
    * <http://fixunix.com/unix/396526-aio_read-write-versus-o_nonblock.html> (lots combustible sarcasm)
    * <http://bouliiii.blogspot.com/2011/10/disk-asynchronous-io-mess-on-linux-and.html>
    * <http://www.bullopensource.org/posix/>
    * Seems the answer is this new AIO API may be better and support things like sockets that file descriptors also did, but it is probably incompatible so you wouldn’t be able to use it in conjunction with existing libraries
    * <http://cr.yp.to/unix/asyncdisk.html> O_HONESTBLOCKING
    * <http://www.baus.net/unix-leaky>
* Files and streams are different.
    * <http://forum.osdev.org/viewtopic.php?f=15&t=13802>
    * Non-blocking _open_() could be like _socket_(AF_VFS, SOCK_STREAM) -> half-duplex socket; _connect_(path, "rb+")

# X Windows #

* Everything seems slow and unsynchronised. Is this the underlying protocol or just a sloppy implementation? Specific example: copy from Firefox, paste into terminal and type something after paste; typing appears before paste completes.
* Copy and paste seems to rely on a originating process running. Kill the process and your “clipboard” disappears. But if this is indeed the case, how does the “xclip” program work?
