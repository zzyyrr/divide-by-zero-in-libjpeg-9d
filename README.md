# divide-by-zero-in-libjpeg-9d
Divide-by-zero in libjpeg-9d

There is a divide-by-zero error in libjpeg-9d.

How to reproduce: cjpeg id_000000,sig_08,src_001010,op_havoc,rep_8

The information from gdb:
(gdb) bt
#0  0x000000000040bfb5 in alloc_sarray (cinfo=0x7fffffffdc70, pool_id=1, 
    samplesperrow=0, numrows=1) at ../jmemmgr.c:407
#1  0x000000000040329f in start_input_gif (cinfo=0x7fffffffdc70, 
    sinfo=0x624f78) at ../rdgif.c:509
#2  0x0000000000400d34 in main (argc=2, argv=0x7fffffffdfd8) at ../cjpeg.c:626


The debug information:
zyr@zyr-virtual-machine:~/CVE/crashs/cjpeg/c1$ gdb /home/zyr/CVE/latest-programs/cjpeg 
GNU gdb (Ubuntu 9.1-0ubuntu1) 9.1
Copyright (C) 2020 Free Software Foundation, Inc.
License GPLv3+: GNU GPL version 3 or later <http://gnu.org/licenses/gpl.html>
This is free software: you are free to change and redistribute it.
There is NO WARRANTY, to the extent permitted by law.
Type "show copying" and "show warranty" for details.
This GDB was configured as "x86_64-linux-gnu".
Type "show configuration" for configuration details.
For bug reporting instructions, please see:
<http://www.gnu.org/software/gdb/bugs/>.
Find the GDB manual and other documentation resources online at:
    <http://www.gnu.org/software/gdb/documentation/>.

For help, type "help".
Type "apropos word" to search for commands related to "word"...
Reading symbols from /home/zyr/CVE/latest-programs/cjpeg...
(gdb) set args ./id_000000,sig_08,src_001010,op_havoc,rep_8 
(gdb) b 626
Breakpoint 1 at 0x400d23: file ../cjpeg.c, line 626.
(gdb) r
Starting program: /home/zyr/CVE/latest-programs/cjpeg ./id_000000,sig_08,src_001010,op_havoc,rep_8 

Breakpoint 1, main (argc=2, argv=0x7fffffffdfd8) at ../cjpeg.c:626
626	../cjpeg.c: No such file or directory.
(gdb) n
623	in ../cjpeg.c
(gdb) n
626	in ../cjpeg.c
(gdb) s
start_input_gif (cinfo=0x7fffffffdc70, sinfo=0x624f78) at ../rdgif.c:391
391	../rdgif.c: No such file or directory.
(gdb) b 509
Breakpoint 2 at 0x403287: file ../rdgif.c, line 509.
(gdb) c
Continuing.
Bogus char 0x0a in GIF file, ignoring

Breakpoint 2, start_input_gif (cinfo=0x7fffffffdc70, sinfo=0x624f78)
    at ../rdgif.c:509
509	in ../rdgif.c
(gdb) s
alloc_sarray (cinfo=0x7fffffffdc70, pool_id=1, samplesperrow=0, numrows=1)
    at ../jmemmgr.c:399
399	../jmemmgr.c: No such file or directory.
(gdb) n
407	in ../jmemmgr.c
(gdb) display samplesperrow 
1: samplesperrow = 0
(gdb) n
399	in ../jmemmgr.c
1: samplesperrow = 0
(gdb) n
408	in ../jmemmgr.c
1: samplesperrow = 0
(gdb) n
407	in ../jmemmgr.c
1: samplesperrow = 0
(gdb) n

Program received signal SIGFPE, Arithmetic exception.
0x000000000040bfb5 in alloc_sarray (cinfo=0x7fffffffdc70, pool_id=1, 
    samplesperrow=0, numrows=1) at ../jmemmgr.c:407
407	in ../jmemmgr.c
1: samplesperrow = 0
(gdb) n

Program terminated with signal SIGFPE, Arithmetic exception.
The program no longer exists.
