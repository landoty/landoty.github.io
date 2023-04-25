---
layout: post
title: Pwn.College Reverse Engineering
date: 2023-04-25
description: uploading some previous solutions from Pwn.College
tags: ctf
---
### Challenge 1.0

- The challenge compares a user-input "license key" with the expected value. 

- The output of the program gives us insight into how our input is formatted and compared against the expected value

    <img src="/assets/img/blog/pwn-rev1-1.png" class="img-fluid">
       
- As we can see, the first 5 ASCII characters are consumed, converted to hex, and compared against the expected value

- We can simply convert the expected hex values to ASCII and supply as input to the challenge

<details><summary><b>Solution</b></summary>
<p>

```bash
    $ python -c "print(chr(0x75)+chr(0x6d)+chr(0x6e)+chr(0x75)+chr(0x64))" | /challenge/babyrev_level1.0
```
</p>
</details>
    
### Challenge 1.1

- This challenge is still comparing an user-input "license key" against the expected value. 

- However, we are no longer provided any information about how our key is processed and what the expected value is...time to reverse!

1. **Debugging Symbols** => None :/

    <img src="/assets/img/blog/pwn-rev1-2.png" class="img-fluid">

2. **Strings** => ;)

    - Using the *strings* command and grep'ing for a 5-length string, we get something promising...

       <img src="/assets/img/blog/pwn-rev1-3.png" class="img-fluid">

    - Let's modify this command to just pipe to the challenge binary:

        <details><summary><b>Solution</b></summary>
        <p>
        
        ```bash
            $ strings /challenge/babyrev_level1.1 | egrep -E "^z.{4}" | /challenge/babyrev_level1.1
        ```
        </p>
        </details>

3. **BONUS: .data**

    - We can also dump the data section to read the license key (just as *strings* does)
    
        <img src="/assets/img/blog/pwn-rev1-4.png" class="img-fluid">