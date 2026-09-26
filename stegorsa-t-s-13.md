# StegoRSA - Cryptography


## Approach
1. I downloaded the image and flag.
2. I uploaded the image on exif and got a suspiciously long comment. It was actually a long hex string.
3. Then I decrypted it using the cryptography break system packages with the help of python.

## Solution
   <img width="1165" height="951" alt="image" src="https://github.com/user-attachments/assets/435462bd-99b7-42a8-8707-58c113103147" />

The comment is a long hex string containing pairs of hex digits. I saved these into a file named comment_hex.txt and to convert it into ints original bytes, I ran the following code in the terminal:
python3 -c "open('private_key.pem','wb').write(bytes.fromhex(open('comment_hex.txt').read().strip()))"

This code reads the comment_hex.txt (read()) and first removes any extra space characters (strip()). Then it walks through the string two characters at a time, and converts them back into their actual bytes. It writes the PEM in binary mode (wb)

<img width="1440" height="684" alt="image" src="https://github.com/user-attachments/assets/eb7211a6-0400-42db-9d16-50f4bf69cc36" />

Thus, the long string turned out to be the ASCII values of a PEM private key file. And we see a readable base64 encryption as the private key.

I then installed the cryptography break system packages, which easily let python use RSA and decrypt with it.
<img width="1844" height="565" alt="image" src="https://github.com/user-attachments/assets/6ad5ecc5-606d-4c36-bcd6-16300a99a23c" />

Python code was:

<img width="704" height="297" alt="image" src="https://github.com/user-attachments/assets/7ccec16f-17e6-4612-bc3f-7720ae6e45a1" />

The code import pieces of the crypto library, which are serialisation (for reading and writing keys in standard forms like PEM here) and padding.
It opens the PEM in binary mode (rb) and loads the raw bytes in a function load_pem_private_key which it then reads. The password is set as equal to none just to say that the key is not encrypted with a password.
Then it reads the flag.enc (shared in the challenge question) as a binary file (rb). And it reads as ciphertext (ct)

RSA representation is simply modular exponentiation: plaintext_number = ciphertext_number ^ d mod n (using private exponent d and modulus n), but it is usually unsafe to use as raw RSA, which is why in real life, it is wrapped with a padding scheme before encrypting. Thus, the code needs to unwrap it as well after decryption.
PKCS1v15() is simply the scheme with which the ciphertext was padded. If it is wrong (like OAEP came out wrong for me), it shows an error.

Finally, it prints the the plaintext, which came out to be the raw flag text.

## Flag - academy{rs4_k3y_1n_1mg_9db27b2c}
<img width="376" height="102" alt="image" src="https://github.com/user-attachments/assets/c6daaa9d-6d65-4af7-9017-512bba02254c" />


## Takeaway
Metadata can often tell a lot about an image, and many information can be hidden inside it, which can easily be viewed using EXIF. And cryptography decryption is often much easier if we install crypto break packages as python can then load RSA and directly decrypt using it.
