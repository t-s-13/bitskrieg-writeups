# Secure Password Database - Reverse Engineering


## Approach
1. I ran the instance, and it asked me to set a password for my account.
2. Once I did that, it asked me how many bytes in length is my password.
3. And then finally it asked me the hash to access my account.

I did this multiple times with different passwords, and got different 'successfully stored password' each time, each trailing with zero's.

## Solution
Since I got each password trailing with zero's, I then put a short password, but entered a long byte length of password. Upon doing this, I got some other characters apart from zero somewhere in the center, again followed by more trailing zero's.

<img width="1843" height="224" alt="image" src="https://github.com/user-attachments/assets/15e9e115-1367-4962-a4b9-e5b14c294205" />

I then considered these characters to be ASCII values, and converted them into text and got the following string: iUbh81!j*hn!

However, when I entered it as the hash, I got the following error:

<img width="669" height="377" alt="image" src="https://github.com/user-attachments/assets/879a1c9b-e36b-4fe1-b0e2-195e8955ac27" />

_________________________________________________________

I then worked on the system.out file which was given in the challenge question.
It required permission to become executable, so I gave the permission using: chmod +x ./system.out

I then tried to view the functions of system.out using: nm system.out and got the following output:

<img width="479" height="910" alt="image" src="https://github.com/user-attachments/assets/97cedf58-e1eb-49a3-b367-28bbbfcccb99" />

Thus I found three functions, titled make_secret, main, and hash.
I tried to understand and decode these using the following objdump command line: objdump -d system.out -M intel --disassemble={function_name}

Thus I got the Disassembly for:
  1. objdump -d system.out -M intel --disassemble=make_secret
  2. objdump -d system.out -M intel --disassemble=hash
  3. objdump -d system.out -M intel --disassemble=main

I also used {objdump -s -j .rodata system.out} to get the read only content of system.out.

Here, with the help of AI, I tried to understand what exactly the program required us to do, and how it needed us to use the found [iUbh81!j*hn!] and turn it into the hash.

Thus, from the disassembly, the 'enter your hash' step does:

<img width="1003" height="154" alt="image" src="https://github.com/user-attachments/assets/314de6c6-547c-42b1-8e11-bad1bc4cae78" />

And the hash() function defines;

<img width="723" height="127" alt="image" src="https://github.com/user-attachments/assets/ba77b23b-5bbe-407b-ae4f-7a0e604ad4c1" />

Thus, using the following python code, we ran the same dib2formula using the 64-bit insigned wraparound.

<img width="727" height="225" alt="image" src="https://github.com/user-attachments/assets/392fc279-5eec-45d2-9a10-2b904b4978e2" />

Running each character theough that loop:

<img width="849" height="413" alt="image" src="https://github.com/user-attachments/assets/d450ce3d-ef0a-420f-bf82-04cbbd55ae9f" />

And thus, we got the final value, [15237662580160011234] as the hash, and thus got the flag.

<img width="593" height="214" alt="image" src="https://github.com/user-attachments/assets/f3b49886-7fe1-4385-b1c2-3dfa00ee8bed" />

## Flag - academy{d0nt_trust_us3rs}

<img width="345" height="79" alt="image" src="https://github.com/user-attachments/assets/b13b2f64-9256-43c8-b8b0-11937ffa2ea3" />


## Takeaway
Commands such as nm, objdump (and even gdb, though I did not use it) can be used to understand the working and mechanism of a program, and accordingly, code can be written to reverse engineer that program, and collect the required information to get the flag.
