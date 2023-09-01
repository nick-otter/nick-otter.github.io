---
layout: post
title:  "How Python Works"
categories: python
---

# How Python Works
{: style="text-align: center"}

From [Brij kishore Pandey](https://www.linkedin.com/posts/brijpandeyji_python-softwareengineering-programming-activity-7089929874324267008-34BG/?utm_source=share&utm_medium=member_android).

Let's walk through a simplified guide to understanding how your Python code becomes a functioning program:

1️⃣ 𝗪𝗿𝗶𝘁𝗶𝗻𝗴 𝘁𝗵𝗲 𝗖𝗼𝗱𝗲: We start with typing out our Python program in a text editor, saving it as a '.py' file.

2️⃣ 𝗣𝘆𝘁𝗵𝗼𝗻 𝗜𝗻𝘁𝗲𝗿𝗽𝗿𝗲𝘁𝗲𝗿: When we run the program, it's sent into the Python Interpreter, which is made up of two parts:

𝗖𝗼𝗺𝗽𝗶𝗹𝗲𝗿: This part changes our Python code into something called byte code. This byte code is saved in a '.pyc' file, helping our program run faster the next time.

𝗣𝘆𝘁𝗵𝗼𝗻 𝗩𝗶𝗿𝘁𝘂𝗮𝗹 𝗠𝗮𝗰𝗵𝗶𝗻𝗲 (𝗣𝗩𝗠): The PVM uses the byte code and follows the instructions one by one until the program is finished or runs into an error.

3️⃣ 𝗟𝗶𝗯𝗿𝗮𝗿𝘆 𝗠𝗼𝗱𝘂𝗹𝗲𝘀: If our program uses library modules from Python's standard library or elsewhere, these are also changed into byte code. The PVM then uses this byte code, allowing our program to use the features of these modules.

4️⃣ 𝗙𝗿𝗼𝗺 𝗕𝘆𝘁𝗲 𝗖𝗼𝗱𝗲 𝘁𝗼 𝗠𝗮𝗰𝗵𝗶𝗻𝗲 𝗖𝗼𝗱𝗲: The PVM further converts the byte code into machine code, which is a series of 1s and 0s. This machine code is what your computer's brain, the CPU, can directly understand.

5️⃣ 𝗥𝘂𝗻𝗻𝗶𝗻𝗴 𝘁𝗵𝗲 𝗣𝗿𝗼𝗴𝗿𝗮𝗺: After the machine code is ready, your computer uses it to run your program. And there you have it! Your Python program is up and running.

Understanding the transition from Python code to a running program provides insight into how Python, as an interpreted language, bridges the gap between the high-level code we write and the low-level operations performed by a computer.

![](/assets/python_works.png)

---