# Binry Digits - Forensics

## Approach
1. I started with the command 'file digits.bin'
   <img width="803" height="80" alt="image" src="https://github.com/user-attachments/assets/57d585a9-3175-4489-a10f-74c9b87b2ea1" />

   I thus found that the file actually contained ASCII text only. Thus, the file content only seemed binary (since it contained only 0s and 1s.) So I needed to convert the very long string of 0s and 1s into actual bytes (8 bits at a time).

3. I thus got a code written from AI which would do this conversion, and write out all the bytes into a new image called recovered.jpg.

## Solution

 <img width="803" height="80" alt="image" src="https://github.com/user-attachments/assets/57d585a9-3175-4489-a10f-74c9b87b2ea1" />

 Thus, code written after this for the conversion into bytes: <img width="661" height="176" alt="image" src="https://github.com/user-attachments/assets/9f61d253-1414-4fe1-a614-15f3b2764fc1" />

 The code first reads the entire digits.bin file and removes any space characters from it. Then the code basically runs a loop, taking 8 strings at a time and converting each group from binary to a byte value.

 I then ran 'python3 ctf.py' in the terminal, and a new file called recovered.jpg was formed in the same folder.
 This image clearly displayed the required flag.


## Flag - picoCTF{h1dd3n_1n_th3_b1n4ry_8d00e35f}
<img width="661" height="176" alt="image" src="https://github.com/user-attachments/assets/daee267c-9756-4f43-91b2-535f6983e8b7" />

## Takeaway
Always check 'file {file name}' first.
