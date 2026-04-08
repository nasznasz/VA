WEEK 4 : Metadata and File Analysis Report

1. Ocean.jpg
<img width="664" height="466" alt="image" src="https://github.com/user-attachments/assets/2b52eeaa-0171-4324-a7e9-c1679c380f17" />

 
I analyzed ocean.jpg to see if any flags were hidden within its background data. I used an online tool called exif.tools to browse the file's metadata. By inspecting the output, I found a specific field labeled "Comment". This section explicitly contained the string "THIS IS THE HIDDEN FLAG," which served as the first discovery.








2. dog.jpg
 
<img width="480" height="620" alt="image" src="https://github.com/user-attachments/assets/4da0a572-18eb-4a3a-90a2-25d2a6c985f0" />

The next task involved dog.jpg, which I analyzed using the Linux command line on a Kali Linux machine. First, I ran the binwalk command to scan the image, which revealed a hidden Zip archive and a file named hidden_text.txt embedded within the image data. To access this, I used binwalk -e to extract the hidden files into a new directory named _dog.jpg.extracted. After navigating into that folder and using the ls command to confirm the files, I used cat hidden_text.txt to read the contents. This successfully revealed the hidden flag: "THIS IS A HIDDEN FLAG".


3. Solitaire
 <img width="507" height="373" alt="image" src="https://github.com/user-attachments/assets/2105763c-1dfd-43bf-a33b-0b915b7a16a2" />

For the third file, solitaire.exe, the goal was to verify if the file was actually an executable. I used the file command in the terminal to check its true signature. The system reported that the file was actually PNG image data with a resolution of 640 x 449, proving the .exe extension was a decoy. To fix this, I used the mv command to rename it to solitaire.png. Finally, I opened the file using the ristretto image viewer to see the actual image of playing cards.
<img width="357" height="271" alt="image" src="https://github.com/user-attachments/assets/3d1a98c9-ef87-46b2-8391-e19bd3b609cc" />

                              


4. Rubiks
 <img width="485" height="264" alt="image" src="https://github.com/user-attachments/assets/8076fc06-0464-4976-b6b7-26db78ef0f98" />

The analysis of rubiks.jpg followed a similar process to identify another extension mismatch. When I ran the file command on it, the output confirmed that it was also PNG image data rather than a JPEG. I used the mv command to change the extension to .png to match its true format. Once the extension was corrected, I was able to open it with the ristretto tool, which displayed a 3D graphic of a Rubik's Cube.
<img width="378" height="396" alt="image" src="https://github.com/user-attachments/assets/a3c0a702-cf23-461d-872e-459bb36e4d86" />

 


5. computer.png
<img width="687" height="261" alt="image" src="https://github.com/user-attachments/assets/f30f9106-4cca-47fe-8ee2-fd396c733a2c" />

The final part of the lab involved computer.jpg, where I needed to inspect the raw binary data. I first uploaded the file to hexed.it, a web-based hex editor, to view the file headers and hexadecimal values.
 <img width="687" height="339" alt="image" src="https://github.com/user-attachments/assets/0f0df759-6ed1-4e9c-958a-a6eabf1153d9" />

To make the data more readable, I also used the dCode Strings Extractor tool. By processing the file through this tool, I was able to pull out human-readable text strings from the binary code, such as ICC_PROFILE and mntrRGB XYZ. This demonstrated how a combination of hex editing and string extraction can be used to analyze a file's internal structure.
