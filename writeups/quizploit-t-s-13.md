# Quizploit - Binary Exploitation


## Approach
1. Downloaded the source code and binary
2. Connected with the challenge instance, and saw that there were 13 questions based on the binary that I was supposed to answer.
3. I used different commands on the terminal to obtain the required answers.

## Solution
Connected with the instance.
Hint fot the first question said to check if system is x86_64 or x86. THus, I did 'file vuln' onnthe terminal and got the answer:
  <img width="812" height="95" alt="image" src="https://github.com/user-attachments/assets/3d98e8b9-3deb-4fd1-b1a2-f0cf46c2485d" />

Thus I got the answers of the first three questions:
  1. 64-bit
  2. dynamic
  3. not stripped

To geth the answer of 4th answer, which asked the size of the buffer in the vuln() function in bytes: I read the raw code of the vuln.c file and got the size from the declaration of the function.
<img width="684" height="559" alt="image" src="https://github.com/user-attachments/assets/35cc38ab-a51e-4272-b021-e671376a1166" />

I thus also got the answer to "how many bytes are read into the buffer?", which was mentioned inside the fgets in the vuln() function. The stanard 'c' function which could cause a buffer overflow in the provided C code is also 'fgets'. And the code here is vulnerable to buffer overflow.

  4. 0x15
  5. 0x90
  6. yes
  7. fgets
  8. win (also found simply by looking at the raw source code)

For the 9th question, "What type of attack could exploit this vulnerability?", I just go the hint that it should be 'buffer overflow' since the term was used in many questions and hints earlier.
<img width="788" height="114" alt="image" src="https://github.com/user-attachments/assets/a8ab0199-c324-4631-ab17-b55f6e0bbabc" />
  
  9. buffer overflow
  10. 0x7b
      --> I converted 0x90 (144) and 0x15 (21), and then subtracted them and converted back into hexa (123 in hexa is 0x7b)

The hint for the 11th question mentioned that i should learn to use checksec. So I installed checksec using 'sudo apt install checksec', and then just typed checksec into the command prompt. It gave me some options and a format to use. The very first option was '--file={file}', so I ran 'checksec --file=vuln' and found NX protection enabled.
<img width="1498" height="737" alt="image" src="https://github.com/user-attachments/assets/d18e4d20-4cdc-4306-9ee6-38c7d5a40c78" />
And to find which exploitation technique to bypass NX, I just used hit and trial method and tried out options till I found the correct answer (hint said to choose from the options).

  11. NX
  12. ROP

For the memory address of win(),  had initially used nm in the terminal and found it. But the hint in the questioned mentioned gdb so I also used gdb to verify the memory address.
<img width="474" height="492" alt="image" src="https://github.com/user-attachments/assets/8f2e4e7d-d8c1-465e-82d7-998b27f6aa53" />
<img width="604" height="779" alt="image" src="https://github.com/user-attachments/assets/bf36ea3f-31ba-4e2a-86ec-8b2a265f18de" />

  13. 0x401176

## Flag : academy{my_bIn@4y_3xpl0it_fL@g_8f2b51f7}
<img width="480" height="231" alt="image" src="https://github.com/user-attachments/assets/10bada06-495a-4242-818a-0b1163d32615" />

## Takeaway
Many questions can be answered simply by understanding the hints (and context), and commands such as 'file, nm' can help us find the properties and memory addresses of various functions. Checksec and gdb (GNU DeBugger) are 2 more tools that are easy and fast to use.
