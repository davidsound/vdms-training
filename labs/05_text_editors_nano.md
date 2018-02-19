## Lab 5 - Command line text editors

In this lab you will learn how to use common, inbuilt command-line text editors from the *nix shell.


### Task 1 - Using `cat` to create and update files

In the previous lab you learned how to use the `cat` command to view the contents of a file. `cat` is a versatile command that also allows you to create and update files.

##### Step 1 

Use the `cat` command to create a new file called `change_notes.txt`. This file is being used to track virtual interfaces that needs to be added to `nyc-rtr01.cfg`.


```
[ntc@ntc ~]$ cat > change_notes.txt
Interfaces to add to NYC-RTR01:
==============================
1. Loopback 1  
2. Tunnel 101
3. Portchannel 10
[ntc@ntc ~]$ 

```

> Use the `CTRL-D` key combination to commit the changes. 


##### Step 2

Confirm that the new file was created and contains the data added in the previous step:

```
[ntc@ntc ~]$ ls -ltr
total 16
drwxrwxr-x. 3 ntc ntc  51 Jan 22 19:56 configs
drwxrwxr-x. 3 ntc ntc  45 Jan 23 17:02 VirtualBoxVMs
-rw-rw-r--. 1 ntc ntc   0 Jan 25 16:01 test
-rw-rw-r--. 1 ntc ntc   0 Jan 25 16:06 error
-rw-rw-r--. 1 ntc ntc 225 Jan 25 16:11 output
-rw-rw-r--. 1 ntc ntc  92 Jan 25 17:20 greetings.txt
-rw-rw-r--. 1 ntc ntc   2 Jan 25 18:00 file_count.txt
drwxrwxr-x. 3 ntc ntc  50 Jan 26 11:03 tux
-rw-rw-r--. 1 ntc ntc 109 Jan 26 17:01 change_notes.txt
[ntc@ntc ~]$ 

```

```

[ntc@ntc ~]$ cat change_notes.txt 
Interfaces to add to NYC-RTR01:
==============================
1. Loopback 1
2. Tunnel 101
3. Portchannel 10
[ntc@ntc ~]$ 


```


##### Step 3

Add interface `Loopback 2` to the `change_notes.txt` file.

```
[ntc@ntc ~]$ cat >> change_notes.txt
4. Loopback 2
[ntc@ntc ~]$ 

```

```
[ntc@ntc ~]$ cat change_notes.txt 
Interfaces to add to NYC-RTR01:
==============================
1. Loopback 1
2. Tunnel 101
3. Portchannel 10
4. Loopback 2
[ntc@ntc ~]$ 

```


> **NOTE: Remember that if you use the `>` vs the `>>` the file will get overwritten



### Task 2 - Using NANO

`nano` is an add-on command line editor (pre-installed on your lab VM) that is easy to use. 

##### Step 1 

Use `nano` to open the `change_notes.txt` file created in the previous task:

```
[ntc@ntc ~]$ nano change_notes.txt

```

This opens the file within the text editor. Note that the menu options for the editor are at the bottom.

```
                                                         [ Read 6 lines ]
^G Get Help          ^O WriteOut          ^R Read File         ^Y Prev Page         ^K Cut Text          ^C Cur Pos
^X Exit              ^J Justify           ^W Where Is          ^V Next Page         ^U UnCut Text        ^T To Spell

```

##### Step 2

Remove the last line from the file. Simply navigate to the line using the `ARROW` keys and then use the `DELETE` key to delete the line.



##### Step 3

Exit the editor by pressing the `CTRL-X` key combination. You are now prompted to save the modifications. Select YES to save by typing the `Y` key.

You are then prompted with the file name to save the contents to. By default it is the same name as the file you opened. 

Typing the `RETURN` key will save and exit the file.


##### Step 4


Create a new file using nano. Call this `ios_config_template.txt`.

```
[ntc@ntc ~]$ nano ios_config_template.txt
```



##### Step 5

Copy  the following lines into your clipboard buffer.

```
interface Loopback 1
  description "Loopback 1 interface"
  ip address 10.1.1.1 255.255.255.255
  no shut
```




##### Step 6

Paste it into the nano buffer by clicking within the terminal window and using the key combination `CTRL-SHIFT-v`

> Note: There is no `undo` option within `nano`



##### Step 7

Use the `CRTL-n` keys to move to the first line. Here, type `CTRL-a` to move to the beginning of the line. 



##### Step 8

Use the `CTRL-g` keys to invoke the help menu to see the different options available within the editor.



##### Step 9

Go ahead and use the `ALT-d` keys to count the number of lines, words and letters in the buffer.


##### Step 10

Change the interface name  and description from `Loopback 1` to `Tunnel 101`. Use the `ALT-r` keys to invoke the search and replace function within the editor:

```


Search (to replace) []:                                                                                        
^G Get Help               ^Y First Line             M-C Case Sens             M-R Regexp                ^N NextHstory
^C Cancel                 ^V Last Line              M-B Backwards             ^P PrevHstory             ^R No Replace

```


Type `Loopback 1` and press `ENTER`. You will be prompted with `Replace with:`. Here, type `Tunnel 101`


You will now be prompted to either replace a single instance or all instance of the match. Go ahead and select `all`


```


Replace this instance?                                                                                                            
 Y Yes           A All
 N No           ^C Cancel

```


Observe that the buffer has been modified and looks as follows:


```
interface Tunnel 101
  description "Tunnel 101 interface"
  ip address 10.1.1.1 255.255.255.255
  no shut

```





##### Step 11
Using `CTRL-o` save the file as `ios_config_template.txt`. Exit the file using `CRTL-x`



##### Step 12

Use nano to open the `/etc/services` file. Use the `ALT-/` key combination to go to the very last line of the file. 



##### Step 13

Use the help menu to find the key combination required to go back to the first line of the file.




##### Step 14

Using the `CTRL-v` and `CTRL-y` keys, scroll through the file, forwards and backwards.


    
##### Step 15

Use the `CTRL-SPACE` and `CTRL-ALT-SPACE` key combinations to go forwards and backwards one word at a time.



##### Step 16

Use `CTRL-x` to exit without saving the file.


