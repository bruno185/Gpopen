GPOPEN (Get_prefix and open)

Is an assembly program for Apple // and ProDOS, using MLI calls.
It explains how to open a file when the prefix is not set (enpty).

The ProDOS 8 Technical Reference Manual says at page 53 SET_PREFIX:
When ProDOS is started up, the system prefix is set to the name of the volume in the startup drive.

That not always true, according to my experience : 
When you boot disk containig PRODOS and BASIS.SYSTEM, the prefix is empty.
(Unless you set prefix manually, typing PREFIX /MYVOL at prompt).

The problem in this case is you can't open a file with just its name with a openfile MLI call. If you call openfile, you will get a $40 error.
My guess : ProDOS can't find the file, since it concatenate the prefix (empty in this case) with the filename to get the full filename.


This program is a solution to open a file safely, with just its name, wether the prefix is empty or not.

To do this my program :
- first check if the prefix is empty with get_prefix MLI call.

- If the prefix is empty, it extracts it at memory address $280, and calls set_prefix.
- Then it can calls openfile, then closefile, without error.

- If the prefix is not empty, it can open and close a file without any error.

Practically :
Boot gpopen.po disk image with Applewin
Brun my program with -GPOPEN command
--> Prefix is empty, but caught at $280.

Reboot
This time, type PREFIX /BRUNO (name of the volume)
Then, brun my program with -GPOPEN command
Now, the prefix is found by get_prefix MLI call.
And used to openfile.

Same behaviour on my Apple //c.
I uses MERLIN32.exe to compile on PC and Merlin-8 on Apple //