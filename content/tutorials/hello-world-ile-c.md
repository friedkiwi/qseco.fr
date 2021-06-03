---
title: "IBM i 'hello world' in ILE C"
date: 2019-05-28
draft: false
---


This tutorial will guide you through writing a simple Hello World application in ILE C and running it.

Requirements:

- Basic C knowledge
- TN5250 Client
- IBM i V5R4 or Higher
- Compilers and dev tools installed on the i box.

# Create Source Physical Files

Source code files on IBM i are stored in special kinds of Physical Files. These files have source members, which would effectively be your individual source files. IBM i does not offer subdirectories in these so this is a flat directory structure.

To create a source physical file, you use the `CRTSRCPF` command. Use F4 after entering the command to open the prompt screen:

![crtsrcpf](crtsrcpf.gif)

For this tutorial, we are creating the source physical file `HELLOC` in the library `YVANJ` (which is the default library for my user profile).

# Write / enter code

For the purpose of this tutorial we are going to use the built-in IDE called `Program Development Manager`. You can start this IDE using the `STRPDM` command:

![strpdm](strpdm.gif)

We continue by selecting `3. Work with members` and respond with the data of the source physical file we just created:

![wrkmbr](wrkmbr.gif)

You will be greeted with the contents of your source physical file, which should be empty:

![wrkmbr2](wrkmbr2.gif)

Hit F6 on your keyboard (Create), and fill out the form as below:

![strseu](strseu.gif)

Hit Enter to confirm.

You will be greeted with SEU, the Source Entry Utility. Enter the following code snipped and make sure to not press enter, just change your cursor position with the arrow keys:

![hw](helloworld.gif)

After filling out the snippet, hit enter, and then F3.

![exitcode](exitcode.gif)

Keep these defaults, and press enter again. You have now created your first C code file on IBM i:

![wrkmbr3](wrkmbr3.gif)

# Compiling the code

C code on IBM i is compiled using the `CRTBNDC` command. Back out of the screen you are currently at by repeatedly hitting F3 until you’re back on the main menu. Issue `CRTBNDC` and finish with F4 to open the auto-complete form as before:

![crtbndc](crtbndc.gif)

Program is the output binary, and the indented library below it is the library you want to store your executable. In my case, this is a binary with the name `hello` in the `yvanj` library. The next three parameters denote the source physical file and the source member to compile; we use the `HelloC` source (physical) file we created earlier and specify the library in which we created it. Then we specify the source member we created using the F6 command earlier. We submit this form using the Enter key.

After a short while, the following status line should become visible:

![compilesuccess](compilesuccess.gif)

# Run the code

Running our application is fairly simple, you can call a program by issuing `CALL LIBRARY/PROGRAM`. In our case this would become `CALL YVANJ/HELLO`

This will result in the following output:

![hellorun](hellorun.gif)

