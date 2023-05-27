---
layout: post
title: Hack the Box - Exatlon Writeup
date: 2023-05-26
description: Challenge writeup
tags: ctf
---

### Initial Analysis

Download the challenge materials from Hack the Box, create a directory, extract, ready to go. We get a single executable, *exatlon_v1*. Run *file* to see what we're working with.

<img src="/assets/img/blog/htb-exatlon-1.png" class="img-fluid">

No section header AND statically linked?? Let's toss it in IDA and see what's going on...

<img src="/assets/img/blog/htb-exatlon-2.png" class="img-fluid">

Ok, yep, this is a mess. The binary has pretty clearly been compressed or packed. Recently, I've been utilizing IDA's string view instead of the default Unix *strings* command. Let's see what we get.

<img src="/assets/img/blog/htb-exatlon-3.png" class="img-fluid">

Ahhhh, there we go. [UPX](https://github.com/upx/upx), the *"Ultimate Packer for eXecutables"* was used here to compress the binary. After downloading the repo and building from source, UPX also provides a decompress or unpack option. Use this and reload the new binary into IDA.

<img src="/assets/img/blog/htb-exatlon-4.png" class="img-fluid">

### Source Analysis

> **NOTE:** Recall the binary is statically linked, so whichever libraries are included to make it work will be fully compiled into the binary and included in IDA's analysis. 

After loading the decompressed binary, we see all of those imports. Note the name mangling, so we're working with a binary compiled from C++! Thankfully, it's not stripped, so let's find the *main* function and see what's going on.

<img src="/assets/img/blog/htb-exatlon-5.png" class="img-fluid">

If you expand the image above from IDA, you can see that after prompting for the password, a call is made to function *exatlon*, followed by a (wacky) overloaded *==*. Also following the call to *exatlon*, a strange string of numbers. 

Now, I should admit the rabbit hole I went down, trying to understand the various standard libary classes used here (char_traits, basic_string, etc.) to make sense of the equivalence operator. After a bit of frustration, I figured it was time for dynamic analysis...

### Dynamic Analysis in GDB

The workflow here isn't too complicated...set a breakpoint for the instruction following the call to *exatlon*, walkthrough the next few instructions before the *==* operator, and check out the registers. 

When running, just give a single character; I did 'a'.

```
b exatlon(std::__cxx11::basic_string<char, std::char_traits<char>, std::allocator<char> > const&)
run
step
ni 2
info registers
```
At this point, the registers don't look too promising...shouldn't *rax* just have 0x61? If you're running a standard gdb configuration, it might not be too obvious, but thanks to [gef](https://hugsy.github.io/gef/), or *GDB Enhanced Features*, this one jumped out at me pretty clearly.

<img src="/assets/img/blog/htb-exatlon-7.png" class="img-fluid">

What the function *exatlon* is doing, is actually returning a string via a char pointer (char \*). We can confirm this by examining the value of *rax*. Note, I use char \*\* here as *rax* contains a pointer to the string.

<img src="/assets/img/blog/htb-exatlon-8.png" class="img-fluid">

Here, I make an assumption that *exatlon* takes an input string, does some defined computation on it, and returns a string derived from that computation. Then, the input-derived string is compared with the string we see loaded into *rsi* in which the program terminates on a match.

### Solution

So, if we can get the "exatlon-d" representation of *every* ASCII character, we can just map each value in the string being compared to it's ASCII representation. This should get the flag.

1. Get all *printable* ASCII characters

    ```bash
    python3 -c 'for i in range(33,127):print(chr(i), end="")'
    ```

2. Run binary inside gdb, giving the ascii characters as the "password"

    ```
    b exatlon(std::__cxx11::basic_string<char, std::char_traits<char>, std::allocator<char> > const&)
    run
    step
    ni 2
    ```

3. Inspect the value of *rax* to get the "exatlon-d" string

    ```
    x/s *(char **)$rax
    ```

4. Script for solution

    ```python   
    # Printable ascii chars and their representations after going through exatlon function
    ascii_chars = "!\"#$%&'()*+,-./0123456789:;<=>?@ABCDEFGHIJKLMNOPQRSTUVWXYZ[\]^_`abcdefghijklmnopqrstuvwxyz{|}~"
    ascii_chars_exatlon = "528 544 560 576 592 608 624 640 656 672 688 704 720 736 752 768 784 800 816 832 848 864 880 896 912 928 944 960 976 992 1008 1024 1040 1056 1072 1088 1104 1120 1136 1152 1168 1184 1200 1216 1232 1248 1264 1280 1296 1312 1328 1344 1360 1376 1392 1408 1424 1440 1456 1472 1488 1504 1520 1536 1552 1568 1584 1600 1616 1632 1648 1664 1680 1696 1712 1728 1744 1760 1776 1792 1808 1824 1840 1856 1872 1888 1904 1920 1936 1952 1968 1984 2000 2016"

    # Exatlon-form flag found in binary
    flag_exatlon = "1152 1344 1056 1968 1728 816 1648 784 1584 816 1728 1520 1840 1664 784 1632 1856 1520 1728 816 1632 1856 1520 784 1760 1840 1824 816 1584 1856 784 1776 1760 528 528 2000"

    # list-ify each
    ascii_chars = list(ascii_chars)
    ascii_chars_exatlon = ascii_chars_exatlon.split(" ")
    flag_exatlon = flag_exatlon.split(" ")

    ascii_exatlon_dict = dict(map(lambda x,y:(x,y), ascii_chars_exatlon, ascii_chars))

    for char in flag_exatlon:
        print(ascii_exatlon_dict[char], end="")
    ```