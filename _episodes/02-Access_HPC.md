---
title: 'Access the HPC'
teaching: 15
exercises: 25
questions:
  - 'How do I log on to the HPC?'
objectives:
  - 'Successfully log on to the HPC with an SSH client'
  - 'Successfully log on to the HPC with a VNC client'
keypoints:
  - 'Interaction with a HPC system starts by first connecting to a login node.'
  - 'The login node is not the HPC, just an interactive gateway.'
  - 'Command-line (terminal) interaction may be established through Secure Shell (SSH).'
  - 'GUI interaction may be established through VNC.'
---

Interaction with a HPC system is often largely through a command-line interface, through a
terminal. To initiate such interaction, you'll first need to establish a connection to a HPC
system computer.
  
> ## Login to where?
>
> There are a number of HPC systems that are easily available to you, both inside and outside
> of the organisation.  
> Today we'll be making use of a system called \_\_\_ by logging into the address \_\_\_.
{: .callout}
  
> ## A note on computer addresses
>  
> This computer address works in a similar way to a webpage address. A Domain Name System
> (DNS) on our network, or the Internet, resolves the address to find the actual computing entity
> the address was assigned to.  
> The address is made up of two parts: A 'hostname', e.g. "bob", and a 'domain name', e.g. 
> "here.com.au", go together to make a qualified domain name, e.g. "bob.here.com.au".  
> You may also see IP addresses (e.g. 127.0.0.1) used as a different form of computer address.
{: .callout}
  
> ## A note on the login node
>  
> The computer we'll be logging into is not the HPC as a whole. It's a singular computer that we
> call a "login node" which essentially acts as a gateway to the rest of the (much larger) system.
> We'll discuss this further in the next lesson.
{: .callout}
  

# Accessing the HPC
## SSH
  
Likely the most common way of getting a command-line connection to another computer, is through
the Secure Shell protocol, or SSH. An SSH client allows users to securely log onto Linux systems
using encrypted sessions, returning an interactive shell.
  
If you're working from a Linux or Mac system, you likely have easy SSH access available to you.  
1. Open a terminal  
2. Run (modified as required):  
```
ssh -X ident@login-node-address
```
{: .language-bash}
  
On Windows, you'll need to select an SSH Client program to use. There are various freeware or
commercial options to choose from. We'll suggest the ubiquitous option 'PuTTY', which may already
be installed on your computer, or is available at 
<https://www.chiark.greenend.org.uk/~sgtatham/putty/latest.html>
  
After you run PuTTY, enter the login node address in the 'Host Name' space.  Leave the 'Port' as
22 and the 'Connection Type' as SSH, then click 'Open'.
{% include figure.html url="" max-width="40%" file="/fig/putty1.png"
 alt="PuTTY login screen" caption="" %}
The Saved Sessions section of the login screen may also be used, so that you won't have to enter
the Host Name again next usage. Enter the HPC system name in the 'Saved Sessions' field, then 
click 'Save'. You can then quickly open a connection by double clicking the name in the list below.
  

> ## Login to the HPC
>  
> 1. Use SSH to log into the HPC.
> 2. Once connected with a terminal, run the command 
> ~~~
> hostname
> ~~~
> {: .language-bash}
> which tells a computer to print its own name to the screen.
> What computer are you interacting with?
{: .challenge}
  

## VNC 
  
An alternate connection option allows for a full GUI (Graphical User Interface) experience for 
interaction with the connected computer. 'VNC' offers a remote display system, which in turn allows
users to run software with graphics capability on the remote machine. VNC is available from Linux,
Mac and Windows systems.

> ## VNC in CSIRO
>
> CSIRO IM&T provides helper scripts for using VNC from Linux or Mac and a graphical interface,
> 'SC_Launcher', for easy connection from Windows.  
> See here ___ for instructions.
{: .callout}
  
  
> ## Login to the HPC 2
>  
> 1. Use VNC to log into the HPC.
> 2. Once connected, open the 'Home' folder from the shortcut on the remote desktop.
> 3. Click File -> Create Document -> Empty File. 
> 4. Enter "hello.txt", then click 'Create'.
> 5. Double click your "hello.txt" file.
> 6. Type some lines of random text. Click 'Save' and close the text editor window.
> 7. Run the Terminal Emulator program on the remote desktop and run the command:
>    ~~~
>    cat hello.txt
>    ~~~
>    {: .language-bash}
>    The command 'cat' (short for concatenate), prints the contents of a file to the terminal.
> 8. Switch back to your SSH command-line connection from earlier and again run:
>    ~~~
>    cat hello.txt
>    ~~~
>    {: .language-bash}
> 9. As a group, discuss and reflect on where abouts your "hello.txt" file exists.
{: .challenge}


{% include links.md %}
