This was an assignment for an introduction to networking class.

# The Quarter Project

Assignment details.\
\
Client, user interface:\
signing in : 8 byte array “name”, 0 padded to 8 bytes. Send exactly 8 bytes. \
command mode: First byte = '/'\
commands:\
“/r” - list the rooms on the server\
“/j roomname”- joins the given room\
“/l” - lists people in the current room\
“/x” - close the connection and log off the server “/p” name - private chat\
“/q” - end private chat\
“/f” filename - send file\
“/h” - help \
Sample client session:
> ./a.out\
> Enter your name: Tom  //You type Tom\
> Welcome to E-Chat. //From the server\
> /r //You type\
> Hobby, 330 //From the server\
> /j Hobby //You type\
> Hobby: prompt show up with room name\
> Hobby: hello //Prompt with room name\
> Hobby: (Jerry Wrote) hello //From server if Jerry wrote hello\
> Hobby: /l //You type\
> Hobby: Tom //From the server\
> /r //You type\
> Hobby, 330 //From the server\
> Hobby: bye everyone //You type\
> Hobby: /j 330 //You type (notice prompt changes to 330)\
> 330: /l //You type\
> 330: Trump Hana\
> 330: /p Trump //You type\
> Trump: you idiot //You type\
> Trump: that is fake //Trump typed\
> Trump: No it is not //You type\
> Trump: /q //You type to quit private chat\
> 330: //You are back to room 330\
> 330: /p Hana\
> 330: /f myfile //Hana sees "file transferred"\
> 330: /x

for f command, packet format \
–------------------------------------------------------------------------ \
32 bit unsigned | 8bytes | byte, byte, byte, .... \
--------------------------------------------------------------------------\
size of file | file name | data\
 --------------------------------------------------------------------------\
Server-Client transaction\
\
After accept(), the server sends “ok” (2 bytes) to the client. The client upon receipt of “ok” sends the client's user name in 8 bytes. Server sends “ok” (2 bytes) to the client, signaling the client is entered in the client list and ready for the chat session.\
\
Chat session. Server:\
\
For a character string received not beginning with '/', it is within-room broadcast. Else it is a command.\
\
case command\
“/r” //Send room names, "Hobby 330" (if the server has Hobby and 330 rooms)\
“/j roomname” //Send "ok", so the client knows the user is in the "roomname"\
“/l” //Send the names of the users in the current room\
“/x” //Close the connection and log off the server\
“/p name” //Send "ok" to let the client know the user is in private chat with name\
“/q” //Send "ok" to let the client know the private chat with name ended\
“/f filename” //Read file name and data from the client and send the file name and data to the designated client in the above file transfer format\
end case;\
\
All strings must be sent with the terminating 0, except sign in session and file command. For example, “/r” must be send as '/', 'r', and 0.


