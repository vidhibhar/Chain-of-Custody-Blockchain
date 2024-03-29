# Blockchain Chain of Custody
### Vidhi Bhargava


####  Background:
Blockchain technology is used in digital forensics to hold various elements of evidence, log changes made, record evidence being checked in, checked out, and why something was removed. The forensics investigator can then use this evidence to make decisions. This evidence can also potentially be presented in court and assist with the decision of a final verdict in a criminal justice case. 


####  Requirements:
I was tasked with developing software that could create, or read from, a blockchain chain of custody file. This chain of custody file stores the blockchain in a binary file. Each block in the file contains the following information: SHA1 hash of the previous block, timestamp of the current time when that entry was logged, case ID, evidence item ID, state, length of the data, and finally, the data. The previous hash field is allocated precisely 20 bytes. The timestamps are given 8 bytes and are stored in UTC. The case ID must be a valid UUID, is allocated 16 bytes in the block, and is stored as an integer. The item ID is given 4 bytes and stored as an integer. The state field can have the following string values only: INITIAL, CHECKEDIN, CHECKOUT, DISPOSED, DESTROYED, RELEASED. The state field is given 11 bytes in the block. The data length field is a 4-byte integer. Furthermore, the data section is given the length of the data length field (i.e., between 0 and 2^32). 

Seven actions must be implemented and functional in the blockchain chain of custody software. The add action allows the user to add additional evidence items to the blockchain. The new evidence item must be accompanied by the case ID to which it should be associated. If either the case ID or item ID is not specified in the arguments of the CLI command, the program should exit in error. Multiple evidence IDs can be added at once for additional convenience. The checkout action records a blockchain evidence checkout. In the arguments, the evidence ID must be specified. Otherwise, the program will exit with an error. An error should occur if the evidence ID has already been checked out or has not yet been checked in. The check-in action records the evidence being checked into the blockchain. In the arguments, the evidence ID must be specified. Otherwise, the program will exit with an error. An error should result if the evidence ID has already been checked in and is not currently checked out. Using the log action, the user can see a log of all the actions taken on the evidence items (i.e., see the entire chain of custody). The user can specify if they want to see the list in reverse. By specifying a case ID in the arguments, the user can choose to display only entries that match that case ID. Similarly, the user can curate the list by specifying an evidence ID. The user should also be able to specify a specific maximum number of entries that they would like to see. Any of these options can be combined to meet the user's needs. The remove action allows the user to remove an evidence item from the chain. Once the item has been removed, the program should prevent any other action from being performed upon that evidence item. The evidence item ID must be specified in the remove command; otherwise, the program should result in an error. Also, only checked-in items can be removed. If the item the user is trying to remove is not in the CHECKEDIN state, the program should throw an error. The init action is used to create the initial block if one has not already been created. Furthermore, the verify action verifies that all the entries in the blockchain are valid. It will result in an error if this is not the case.

####  Design:
For this project, the chosen programming language for use is Python. Python is a flexible language, allowing it to be used effectively to deal with data processing as is required by this project. The code is simpler to write and understand and enables the development to proceed smoothly. Numerous libraries are available to help simplify the development process, but even the standard libraries contain much helpful code. In this project, the "struct" library was chosen as the primary medium in which the data storage is handled. 

The struct library functionalities were chosen for this project because it contains methods that allow for the packing and encoding of given data strings. A struct object is used to read and write binary data according to the given buffer string, given specific values to help encode things like the header and data for a given block. Given the first time a block is being created, the data is directly added to the data structure. A hash is also created using SHA-1 for each respective block. After a data block is added, checking is done to remove any duplicates. The blockchain is stored as text in a file with utf-8 encoding, which is read back when data is retrieved. The progr_am parses the data by iterating through each entry in the blockchain until it reaches the end, reading the header and data into an array. Data is unpacked from the block with specified byte lengths for each field. 

![alttext](https://github.com/vidhibhar/Chain-of-Custody-Blockchain/blob/4410b2c123ada7081e5d9b88393faa5ce3c57be9/pics/1.PNG)

*Code snippet showing the function to initialize a block*

In order to maintain the integrity of the blockchain, the history of data added or removed should be recorded. State variables are used to define whether data is still checked in or destroyed. Data added is given the initial value of CHECKEDIN to their state, and the current timestamp is included with the header information. Issues with the user input such as missing parameters or incorrect syntax will incur different error codes as output, with exit 0, meaning the program is appropriately executed. 

User input is handled with argparse, a library designed for parsing input from the command line. The various behaviors and valuable information that should be displayed to a user for a given action are defined at the beginning and fed into an if/else statement to determine what should be done.

![alttext](https://github.com/vidhibhar/Chain-of-Custody-Blockchain/blob/4410b2c123ada7081e5d9b88393faa5ce3c57be9/pics/2.PNG)

*Code snippet showing a section of the argument parser for user input*

We parsed the blockchain by reading the correct number of bytes corresponding to the size of each block from the blockchain file, unpacked those bytes, and used namedtuple's ._make() method to parse the data into its corresponding sections (i.e. item_id, case_id, sha1_hash, etc) and wrap them into a namedtuple. We used this method as it is extremely simple syntactically and was the method suggested by the Python docs for the struct library (found here: https://docs.python.org/3/library/struct.html). 

You can say one challenge was figuring out how to parse the unpacked data into each section efficiently, and that we used ._make() as a solution using this as a reference for the syntax:


####  Challenges:
My choice of the programming language used for the project was Python, and with this choice came many struggles, such as the library implementation, especially when dealing with the structures. The process and exact syntax for reading blocks in bytes were also a concern as the data unpacking. I chose the language of Python because it was a language we were comfortable with and had some previous knowledge, and had lots of familiarity with the syntax. The blockchain was parsed by reading the correct number of bytes corresponding to the size of each block from the blockchain file, and then we unpacked those bytes. The method ._make() from the "namedtuples" was used to parse the data and transport it into the appropriate sections(i.e., item_id, case_id, sha1_hash, etc.). The main challenge that can be said with unpacking this data was how to create maximum efficiency when distributing into each section, and the ._make() method incorporates the syntax from the website "https://docs.python.org/3/library/struct.html" we learned the lesson that you must use a formatted buffer unpack tactic. This struct returns the byte values of string format, and the result must be the tuple if it only contains one item, which is crucial for us because of the check-in to see if the block with the previous hash was valid.

![alttext](https://github.com/vidhibhar/Chain-of-Custody-Blockchain/blob/4410b2c123ada7081e5d9b88393faa5ce3c57be9/pics/3.PNG)

*Code snippet showing check functionality for blocks to checkin and checkout*


The bhoc verification and how it will impact the blockchain's function and transactions were the aspects of the process involving the specific syntax we had to choose for the file. The tuple header for this functionality of verify was done using a lot of if statements, and if met, it would update the state information to the current value. The challenge we faced while writing this file was ensuring that the parameters of item_id and path were accurate to the block structure. This function corresponds with the check.py, where both the checkin and checkout are given. The common is imported; the challenge we faced while writing this file was ensuring that item id and path parameters were accurate to the block structure. The lesson learned in the log.py file was that the try and except feature was the best way to get the case id updated to the current, with the current and previous timestamps updated. The data is retrieved in the init.py, and it shows the results if there is an initial block, and if not, it sends the signal to go and create one with the proper specifications. 

![alttext](https://github.com/vidhibhar/Chain-of-Custody-Blockchain/blob/4410b2c123ada7081e5d9b88393faa5ce3c57be9/pics/4.PNG)

*Code snippet within file log.py showing the Tuple Header and case id time updates*

The final challenge that was already predicted to be an obstacle to the given requirements was the environment variable of the Bchoc file path and how the program would fail all grading tests, so it must be handled. Our process accounted for this because all the edge cases were considered. The paths that would result if any of them were activated are demonstrated in elseif statements for each specification (i.e., add, remove, checkin, etc.). This method also accounts for the item_id and why, if it was not present, the message "missing item_it" would result when displaying the syntax error. The main takeaway from keeping all of these checks in check was that it provides the blockchain with all of the information it requires to display when an error occurs and everything it requires for the path to run smoothly.



####  Discussion:
A blockchain chain of custody solution is undoubtedly reasonable in specific industries like criminal justice, commercial transportation, and other industries where the tracking and logging of the custody and location of specific items are vital. This method allows for storing the detailed chronological history of any relevant item, complete with timestamps of when the status of that particular item's status changed (e.g., changed custody, removed from the list, etc.). This blockchain can also be shared with anyone anytime that needs to see the history of any tracked item. The blockchain can be protected with encryption and read/write permissions to maintain its integrity and deny unauthorized access. 

There are certainly cons to the implementation of blockchain technology. Securing the blockchain is not a simple task, and it requires extreme vigilance and expertise. Mistakes can be made, and sensitive data can be leaked or modified by an unauthorized party. A highly skilled, trained professional must be hired to implement the blockchain and maintain its security. This hiring of professionals comes with a high cost which might outweigh the potential benefits of having the blockchain in some contexts.

Therefore, a digital blockchain chain of custody solution, such as was implemented in this project, is a reasonable solution in some contexts. However, it certainly has downsides that must be weighed against the upsides before a solution can be decided upon the blockchain chain of custody. 



