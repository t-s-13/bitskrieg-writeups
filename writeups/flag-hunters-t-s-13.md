# Flag Hunters - Reverse Engineering

## Approach
I ran the challenge instance: "nc chatelaine.cylabacademy.net 26258" in the terminal. It started displaying the lyrics of a song, and stopped to ask for Crowd: .
For the first two-three times, I just typed anything just to see what happens. After that, I broke the connection using Ctrl+C.

I also tried to run the python code using: "python3 lyric-reader.py", but it showed the following error: <img width="686" height="143" alt="image" src="https://github.com/user-attachments/assets/13f6fc08-b6a0-4106-8fda-1b6e64b72f64" />

## Solution
I viewed the code of the python file:

<img width="395" height="233" alt="image" src="https://github.com/user-attachments/assets/30d78afa-516e-4e1c-96e1-4f7111a37302" />

I found a secret_intro titled verse, which never got displayed on the instance, due to the fact that the code was designed in a way that the pointer(lip) never pointed at the particular verse, and didn't capture the flag either (flag mentioned in the index 3).

Most lines in the code are simply song lyrics, which get printed. However, some particular keywords behave as interpreters:

  1. REFRAIN (acts like a function call and jumps to the refrain section)
  2. CROWD (requires user input)
  3. END (tells code to stop)
  4. RETURN X (jumps to that particular line)
       --> thus, we require the code to jump to line 3 (which contains the flag)
     
<img width="379" height="144" alt="image" src="https://github.com/user-attachments/assets/dc053960-9d53-4ec2-ad3a-0ee12d93153e" />

Also, a semicolon ";" is used to split the line after it is processed.
Thus, a semicolon can be used multiple in one line and many lines can be processed within one line of command.

So whenever the code asks the user for input (CROWD), the user is ideally required to write a small code within that one input line of command, which would then force the pointer to point to the index 3 of the song, and the code would print the flag.

<img width="781" height="901" alt="image" src="https://github.com/user-attachments/assets/73213900-c32e-4034-8acd-5e28f379122f" />

## Flag - academy{70637h3r_f0r3v3r_577e16ad}
<img width="589" height="97" alt="image" src="https://github.com/user-attachments/assets/db8e4084-09c8-4587-85ec-dfbff6c39d6c" />


## Takeaway
Reading and understanding the source code can often be an extremely helpful step in finding the flag. Sometimes, the flag is hidden in plain sight, and simply requires the correct user input / prompt to be revealed.
