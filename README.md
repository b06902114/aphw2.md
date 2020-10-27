# sphw2.md

1. 
 **1. Directories and files. Given a UNIX file system, in which** 

 **(a) an i-node has 12 direct pointers, 1 singly-indirect pointer, 1 doubly-indirect pointer, and 1 triply-indirect pointer,**

 **(b) a disk block is 4096 bytes long,** 

 **(c) a block pointer is 4 bytes long, and** 

 **(d) all of the directories are a single disk block long, how many i-nodes and disk blocks would need to be accessed if we want to read the entire file of /home/user/alice.txt? Suppose alice.txt is 5242880 bytes long. Explain your calculation.**

#### Sol:  5242880-bytes / 4096 = 1280
For 12 direct pointer, 48KB.
     For 1 single indirect pointer 4KB/4-byte pointer = 1K * 4KB = 4000KB
	 For 1 double indirect pointer 4000*1000 = 4000000KB
	 So we need to access directories /home and /home/user for 2 single disk block and 48KB+4000KB + 1097728-bytes, which means 2 + 12 + 1000 + 268 + 2(this two for single indirect and double indirect pointers)= 1284 disk blocks we need.
The i-nodes we need are 3 , 1 for /home , other 1 for /home/user and another 1 for alice.txt.

2. 
  **Soft and hard links. Alice and Bob propose a   method to share files securely. Each of them creates two directories OutBox and InBox in their home directories. OutBox is the place to store the files to be shared; InBox is the place to store the links pointing to the shared files.**
**For example, if Alice wants to share her file hw with Bob, she can just put the file in ~Alice/OutBox and then create a link pointing to ~Alice/OutBox/hw in ~Bob/InBox. For security issue, no one is able to traverse both of OutBox and InBox except the owner. Only the owner can remove his/her own files from OutBox. Assume Alice and Bob belong to different groups and they both have no superuser privilege. Please answer the following**

#### sol (a):
To creat a link between file we use **ln** command。
To copy a file we use **cp** command。
The main advantage is the access permission，cuz if we want to change the permission of a file，we only have to do it on the origin。
But if we copy the file，we have to find out all of the file we copy and change it one by one。

#### sol (b):
Obviously for using symbolic link is a wise choice。Because symbolic link can span all file systems as they are simply the filename of another file。Deleting a target file for a symbolic link makes that link useless。Moreover，it can be create for a directory，but hard link can't。
Hard link are only valid within the same file system，and once a hard link has been made，the link is to the i-node。which means deleting、renaming or moving the original file will not affect the hard link。Any changes to the data on the inode is reflected in all files that refer to that inode.

#### sol(c):
**InBox :** '-wx' because Alice or Bob needs to create link in it。And we protect it by other can't read，so other can't ls to read the files in InBox。

**OutBox(hard links adopted) :**
"- - -"，Because the hard link is made，no need for more premission for other。Bob can use link to access file。Even delete original file does not influence Bob。

**OutBox(symbolic link adopted) :**
"- - x" for other， they can cd into the directory，but do not let them remove any file，never gives other write permission， because once rename or remove， symbolic link will be useless。And we do not want to share those files with other，so no need for read premission. 





#### sol(d):
YES,because Bob is the owner of directory InBox，so he has the perimisson of write and execute，which means he can remove the file in the Inbox，no matter what the perission of that file。


