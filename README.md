# Blockchain Chain of Custody
## Vidhi Bhargava

####  Background:
Blockchain technology is used in digital forensics to hold various elements of evidence, log changes made, record evidence being checked in, checked out, and why something was removed. The forensics investigator can then use this evidence to make decisions. This evidence can also potentially be presented in court and assist with the decision of a final verdict in a criminal justice case. 


####  Requirements:
I was tasked with developing software that could create, or read from, a blockchain chain of custody file. This chain of custody file stores the blockchain in a binary file. Each block in the file contains the following information: SHA1 hash of the previous block, timestamp of the current time when that entry was logged, case ID, evidence item ID, state, length of the data, and finally, the data. The previous hash field is allocated precisely 20 bytes. The timestamps are given 8 bytes and are stored in UTC. The case ID must be a valid UUID, is allocated 16 bytes in the block, and is stored as an integer. The item ID is given 4 bytes and stored as an integer. The state field can have the following string values only: INITIAL, CHECKEDIN, CHECKOUT, DISPOSED, DESTROYED, RELEASED. The state field is given 11 bytes in the block. The data length field is a 4-byte integer. Furthermore, the data section is given the length of the data length field (i.e., between 0 and 2^32). 
Seven actions must be implemented and functional in the blockchain chain of custody software. The add action allows the user to add additional evidence items to the blockchain. The new evidence item must be accompanied by the case ID to which it should be associated. If either the case ID or item ID is not specified in the arguments of the CLI command, the program should exit in error. Multiple evidence IDs can be added at once for additional convenience. The checkout action records a blockchain evidence checkout. In the arguments, the evidence ID must be specified. Otherwise, the program will exit with an error. An error should occur if the evidence ID has already been checked out or has not yet been checked in. The check-in action records the evidence being checked into the blockchain. In the arguments, the evidence ID must be specified. Otherwise, the program will exit with an error. An error should result if the evidence ID has already been checked in and is not currently checked out. Using the log action, the user can see a log of all the actions taken on the evidence items (i.e., see the entire chain of custody). The user can specify if they want to see the list in reverse. By specifying a case ID in the arguments, the user can choose to display only entries that match that case ID. Similarly, the user can curate the list by specifying an evidence ID. The user should also be able to specify a specific maximum number of entries that they would like to see. Any of these options can be combined to meet the user's needs. The remove action allows the user to remove an evidence item from the chain. Once the item has been removed, the program should prevent any other action from being performed upon that evidence item. The evidence item ID must be specified in the remove command; otherwise, the program should result in an error. Also, only checked-in items can be removed. If the item the user is trying to remove is not in the CHECKEDIN state, the program should throw an error. The init action is used to create the initial block if one has not already been created. Furthermore, the verify action verifies that all the entries in the blockchain are valid. It will result in an error if this is not the case.

####  Design:

####  Challenges:

####  Discussion:



