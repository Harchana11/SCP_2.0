# SCP_2.0
How I tried and solved the SCP 2.0 challenge for ABOH.

Our version of how we solved it with the use of AI.

Recently, my team participated in the recent annual CTF event that is organized by the Forensic and Cybersecurity Research Centre Student Section-FSECSS committee called, the Asean Battle of Hackers. This event took place on 2nd December 2023 and lasted for a total of 6 hours.

Together we tried solving many challenges and among the ones that we have chosen to do today would be called the SCP 2.0, which was basically one of the hardest ever challenge, and we were not able to solve it immediately. This challenge is categorized under reverse engineering, but it had a tinge of forensic tasks to be dealt in the category.

This challenge came with minimal hints, but one zipped file, which has to be unlocked with a password. The password was given to us by the challenge creators through the common communication platform we had between the participants and the organizers, Discord. Upon unzipping the content, we are led to two other files, one is a usual PDF file with the title ‘Procedure-15A’ and another is a zipped file called ‘memdump.zip’. Hence this is where we knew that the challenge for this particular question, started here.

Tools Used
Volatility: Think of Volatility as a detective tool for computer memory. Just like detectives look for fingerprints and clues at a crime scene, Volatility examines the computer’s RAM (its short-term memory) to uncover hidden details about what the computer was doing at a specific time. It’s particularly good at finding evidence of malware or understanding a system’s activities right before an incident, thanks to its ability to analyze memory dumps — snapshots of the computer’s memory.
ChatGPT: Developed by OpenAI, ChatGPT is like having a highly knowledgeable friend you can chat with about nearly any topic. For our challenge, we used it as a brainstorming partner to get ideas on how to approach problems, understand complex technical concepts, and even debug issues. Its ability to process and generate human-like text makes it an invaluable resource for quickly learning new techniques or getting a second opinion on a tricky problem.
CyberChef: Imagine CyberChef as a digital Swiss Army knife for data manipulation. It allows us to mix and match different tools (called “operations”) to transform data in various ways — decoding messages hidden in complex formats, encrypting or decrypting data, compressing files to make them smaller, and much more. It’s an incredibly flexible tool for quickly performing operations that might otherwise require multiple specialized software tools.
Write Up (How we solved with the help of AI ?)

Battle of Hackers Event description
This was the password that was given to us by the challenge creator via Discord, for us to unzip the main file.


Password required to unzip the file
Upon successfully entering the password “D!D_Y0U_try_scp1.0” into Engrampa to unzip the main file, we discovered two files: a PDF titled ‘Procedure-15A’ and another zipped file named ‘memdump.zip’.


Shows a PDF file
It then proceeded to take us to these two files, as mentioned we had one pdf and another zip file in it.


Shows a PDF file about SCP Foundation article called the Procedure — 15A
This pdf came with 2 pages, hence we got to know that SCP, stands for Secure, Contain and Protect.


Shows the content of the file
Meanwhile, the second page came with a particular case for us to solve it. It says as below:

“All personnel must back up site data including documentation, images, videos and other relevant information to Foundation servers. All evidence and data must then be eradicated from all devices. Please refer to the Lockdown Procedures document for the full documentation for Containment Breaches which follows the lockdown procedure for Site-18. This document is a guidance to all Foundation sites to ensure consistency and that we, as the SCP Foundation, follow our motto. Secure, Contain and Protect.”


The websites link.
Upon clicking on the link for the Lockdown Procedures document, it takes us to a website, https://scp-wiki.wikidot.com/lockdown-procedures.


Shows a raw file in the zip file
Upon clicking the memdump.zip file it shows that there is another file called memdump.raw inside it. It’s another type of zipped file.


hows no suitable program to run the file.
We tried to extract all the documents by clicking it normally but what happened was that it showed that there are no suitable applications found in Kali Linux to open it.


Shows commands of unzipping the file.
First, we tried to unzip the file, however, it kept showing us the file could not be found or that it could be part of one disk of a multi-part archive. The ‘ls’ command helped to show the file that is currently on the device, but it cannot be opened or unzipped.


Shows details of the files.
Then, the ll command was used. The difference between the ls and ll commands would be that the basic ls command provides a simple list of files and directories in the current directory. Meanwhile, the ll command, which is often an alias for ls -l, provides a detailed listing of files and directories, it includes additional information such as file permissions, number of links, owner, group, file size, modification time, and the name of the file or directory.


Shows which tool can be used to solve this.
I googled the question, on what tool that I needed to use for this case, and that is how I got to know about volatility.


Shows a blog on how volatility works.
First, a few websites were read about memory forensics and analysis.


Shows website to install volatality.
The very first step that I did was try and install volatility. However, this particular tool cannot be downloaded from any online resources except for the one which was uploaded to GitHub. After searching multiple times, I managed to find the most suitable link that took be to someone’s repository where they had uploaded volatility. https://github.com/volatilityfoundation/volatility


Shows a https link to install volatility.
Upon clicking onto the link, it took me to the above page. In order for us to clone, we needed the particular link, so I had to click code and copy the HTTPS link to be used in the kali terminal.


Shows installation of volatility.
The link had to be copied, before the command for cloning has to be used alongside the link. Once you do that, you will be able to see where volatility is being cloned into the terminal.


Shows installation of python in volatility directory.
After that, we need to use python, and as referred in the GitHub website, a python version has to be more than 2.6 but not 3. Hence, we tried to set it up accordingly.


Shows installing python script using wget command.
Python was installed the same way, only thing this time we had to access the link from our search engine.


Shows making a file in python script.

Shows a prompt related to python

Shows the information of the plugins
We used the command above to know the framework of volatility used for this.


Shows the details of the files in downloads directory
Since the challenges was already downloaded, we wanted to make sure it was there, so the ll command was once again used and we can see both files there. With the tool volatility installed this time, we were able to unzip the memdump.zip file and it shows us that it managed to inflate meaning it could unzip and get the memdump.raw file which was inside it. However by unzipping the file, an image file of a machine’s RAM is extracted.


Shows the version of python
We then tried to open the memdump.raw file to get the images info but we couldn’t as it kept showing us there isn’t such file. So to make sure we had the right file, we once again checked the version of python that was installed and we got Python 2.7.18.


Shows a question being ask in chatgpt

Shows question being answered by chatgpt
However, we had no idea on how to proceed after that, but what we knew was that we had to locate the file directory using python so that we could get the image’s info. In the above image, it shows that volatility is in the option file.


Shows the prompt in chatgpt

Shows the process of retrieving image
However, based on the help with ChatGPT, I changed the location of my file to where I had my volatility downloaded so the location of the file was changed as shown in the command above.


Shows failure of getting info from the image
Upon launching the request, we received tons of failed response, however towards the end, I did manage to get the info of the memdump.raw image. As you can see it shows multiple details within the info data. By using volatility, the profile of the RAM image that was unzipped earlier showed that it was from a Windows 7 machine. he information provided includes details about the profile that Volatility determined based on the Kernel Debugger Block (KDBG) search, the address spaces, the number of processors, and various other information about the memory image.


Shows installation of package in python
We noticed the absence of some packages in the default repositories might be due to changes in the package management system, or these specific packages may no longer be included in the repositories and that is what causing multiple error messages and to try and fix that we tried downloading a few necessary tools. We started by python-crypto.


Shows a prompt in chatgpt
As, we did not know on what were the other things that was needed for volatility, we tried searching it via ChatGPT where it adviced to download distorm3. Distorm3 takes an instruction and returns a binary structure rather than static text, which is used for binary code analysis.


Shows installation of pycrypto distorm3

Shows the question being asked to chatgpt

Shows the process of downloading python modules.
Pip3 on the other hand is used to download all the python modules.


Shows the websearch on how to get process tree in volatility

Shows a website “eyehatemalwares”

Shows how to get the command from chatgpt

Shows pstree command being used
In order, for us to see what is inside the RAM that was extracted from the memdump.raw earlier, we had to use the pstree command. In a Windows operating system, a process tree otherwise known as pstree illustrates the relationship between various processes, showcasing parent-child associations. This analysis aids in understanding the execution flow and dependencies among processes, providing valuable insights for forensic investigations or malware analysis.


Continuation
However, after this analysis, I noticed a few strange things that were found in this analysis. Among everything, there is a document used, which can be seen in the image above which shows winword.exe which is known as a Microsoft Document.


Shows how we learnt to check if there is a document memory dump RAM

Shows Volatility framework command
Since I knew that it was s document, I knew it uses a grep doc command and to make sure it was correct, I typed the command into ChatGPT before getting the correct answer which is shown in the image below.


Shows grep of the document

Shows ChatGPT command for volatility framework
The command executes the Volatility framework to perform file scanning on a memory dump file (memdump.raw from a Windows 7 SP1 x64 system. The output displays various memory regions associated with files, and the grep doc command filters the results to include only entries containing the term “doc.” The results include memory regions with addresses, permissions, and paths related to documents and only two files were found which were the SCP-055.docp and the SCP-055.doc files. These shows that there is data in the document in the memory dump, which means it is important to perform a forensic analysis related to the document file SCP-055.doc.


Shows output of dumpfile
I want to run the dumpfile plugins, however, I chose to not set a place where I wanted to save it. I also managed to get the number -Q 0x0000000001049070 from the previous details. The address 0x0000000001049070 is the virtual address of the document SCP-055.doc in the memory dump. In the previous output using file scan plugin, the first column provides the virtual address of the file in memory.


Shows output of the file
I received the output of the file from the previous step which provides information about the type and characteristics of the specified file. The format of the output typically includes details such as the file type, encoding and the rest. In this case of reverse engineering and a mix of digital forensic, this is a memory dump case and the file command on a specific file can help identify its nature, such as whether it is a text file, binary file, image, or other types. So now that is what the current command did.


The file is being read
However, the file turns out to be a file filled with data which indicates that non-readable contents may be inside.



Shows question being ask in chatgpt
Based on the output, we needed to know if there was a txt file, enum or curl but had no idea how, so we copied the details into ChatGPT and managed to get the confirmation below.


Shows output of chatgpt

Shows explanation about MFT

Shows where we googled more about MFT

Shows how we randomly clicked more about MFT Parser

Shows command of using mftparser
We tried to find flag.txt by using the filescan plugin and filtering the results with grep, but we got nothing. Then we checked the challenge description and realized that the employee might have deleted all the files on the machine, following the procedure in the pdf file provided. Therefore, the file could be hidden in the $MFT file, which we could extract with the mftparser plugin. We learned this from searching with ChatGPT. So, we followed the advice and used the mft command to save it to the mft file. The command we used is shown below.

python2 /home/harchana/Downloads/volatility/vol.py -f /home/harchana/Downloads/memdump.raw — profile=Win7SP1x64 mftparser > mftparser


Shows a file created in called mftparser
Since the command above saves it to the mftparser, a file is automatically created as specified in the particular location stated in the command. Upon opening it multiple data could be found as shown in the image below.


Shows a recycle pdf file
However, the pdf hints that the user might have deleted the data, so we tried to search for Recycle as the deleted will automatically be found in the recycle bin and only 6 searches were found, most had no data except for the second search which was also a text file and seemed suspicious.


Shows the flag output after being converted
The data was copied and used with cyberchef to decode as it had hex and base 64 values and we managed to get the flag which says ABOH23:C0NT41nm3Nt_Breach_8Y_M@cr0$.

Reflection on the SCP 2.0 Challenge
Teamwork and Collaboration
Embarking on the SCP 2.0 challenge, our team demonstrated the essence of collaboration and shared determination. This experience underscored the importance of diverse perspectives and skills in solving complex problems. Each team member brought unique strengths to the table, allowing us to approach the challenge from different angles. The synergy within our team not only propelled our technical efforts but also bolstered our morale throughout the intense six-hour contest.

2. Technical Skills and Problem-Solving

The SCP 2.0 challenge tested our technical prowess to its limits, weaving together reverse engineering and digital forensics into a labyrinth of tasks that required a meticulous approach to unravel. Utilizing tools such as Volatility, ChatGPT, and CyberChef, we navigated through layers of data extraction, analysis, and decryption. This challenge served as a practical test of our theoretical knowledge, pushing us to apply our skills in a real-world scenario. One of the key takeaways from this challenge was the importance of adaptability in problem-solving. We encountered unexpected hurdles, such as compatibility issues and the need to decipher minimal hints. These obstacles taught us to be resourceful, seeking out novel solutions and learning on the fly.

3. Learning from Challenges

Perhaps one of the most valuable aspects of participating in the SCP 2.0 challenge was the learning derived from the challenges we faced. Notably, the minimal guidance provided taught us the significance of critical thinking and self-reliance. We learned to trust our instincts and leverage our collective knowledge to navigate through uncertain terrains. Additionally, the technical difficulties we encountered, such as the initial inability to access the memdump.raw file, highlighted the necessity of a strong foundational understanding of the tools at our disposal. It was a poignant reminder that in the realm of cybersecurity, continuous learning is indispensable.

4. Looking Forward

Reflecting on this experience, it’s clear that while we have much to be proud of, there is always room for growth. Moving forward, we aim to deepen our technical expertise, particularly in areas that proved challenging during this contest. Engaging in regular practice sessions and embracing a wider range of CTF challenges will be crucial in honing our skills. Moreover, we recognize the importance of enhancing our problem solving strategies, particularly under pressure. Developing a more structured approach to tackling challenges, including effective time management and prioritization, will be a focus for our team.

Conclusion
In conclusion, the SCP 2.0 challenge was not only a test of our technical skills but also a profound learning experience that stretched our capabilities and fostered a deeper bond among our team members. It reinforced the value of perseverance, collaboration, and an unyielding curiosity. As we reflect on our journey, we are reminded of the ever-evolving nature of cybersecurity and the continuous journey of learning it entails. We look forward to facing future challenges with the same zeal and determination, armed with the lessons learned from the SCP 2.0 challenge.

