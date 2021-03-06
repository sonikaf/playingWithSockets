# Project 2: Hangman Game - Not Another Instruction


## Deliverable

You need to implement two separate C/C++ (or Python, Java) programs: one for the server (__server.x__) and another for the client (__client.x__). Note that, here "__server.x__" could be __server.c__, __server.cpp__, __server.py__, or __server.java__, it depends on which language you choose. Same rule applies to "__client.x__".  

+ Server program should accept one **required** input parameter: the port number. For example, if we want the server to listen to port number 2017, we will start your program by doing "./server 2017". Besides that, your server program should accept an **optional** input parameter (Note that, here **optional** does not mean that **you can choose to support it or not, but we can choose to input it or not**): the word dictionary. For example, we might use our own word dictionary to test your code, in this case, we will start your program by doing "./server 2017 words.txt". words.txt would be a text file, the following gives you an example:
    ```bash
    4 10
    jazz
    buzz
    hajj
    fizz
    jinx
    huff
    buff
    jiff
    junk
    quiz
    ```
    The first line contains two integers: the first one is for the word length, and the second is for number of words in the word dictionary. Then the following lines contain the words, where each line contains exactly one. Note that, all these words have the same length.
+ Client program should accept two required input parameters: the server ip and port number. For example, if the server ip is 127.0.0.1 and port number is 2017, then we will start your program by doing "./server 127.0.0.1 2017"

You need to submit one compressed folder containing __server.x__, __client.x__, and a Makefile (if you use C/C++/Java) and some other necessary files. Note that, We will compile your program by just doing “make”, if you use C/C++/Java. Therefore, your final submission should be structured as follows after being decompressed. Do not change the name of required files.

```text 
Hangman
     - server.x (main server source; required)
     - client.x (main client source; required)
     - server (executable file for server; required if you C/C++/Java)
     - client (executable file for client; required if you C/C++/Java)
     - Makefile (required if you use C/C++/Java)
     - README (Explains how to compile and/or run your code and other important things; required)
     - Other .c or .h or .java or .py or requirements.txt (if you use Python) files (optional)
```

Note that, the `executable file` for Java could by either a `jar` file or a `class` file (thanks @Niandong Xu), but you need to specify in `README` the usage of the `executable file`, _i.e.,_ how to execute it.

### Pay Attention

> in the README: 
+ Please give a high level description of your implementation idea.
+ Please include the names of both team members.
+ An explanation of how the work was divided between the team members (_i.e._ who did what)
+ You should explain clearly how to use your codes. You should explain how to install dependent packages for being able to test your code. For example,  if you use Python and use some packages that are not built-in packages, you might need create a requirements.txt file that contains all such packages.  
+ You should include a test result:  
    - word dictionary (_i.e.,_ the 15 words) used
    - messages printed by server and client during the program flow  
    Note that, you could use separate files to store these information (_e.g.,_ word_dictionary.txt, server.txt, and client.txt) and explain in README which file is which information.
    
> in the source code:
+ Please provide necessary comments


> All messages that are between server and client should strictly follow the message format described in the instruction, for example,  
+ Server overload message: message header + "**server-overloaded**", _i.e.,_  

    | Msg flag 	| Data[1] 	| Data[2] 	| Data[3] 	| Data[4] 	| Data[5] 	| Data[6] 	| Data[7] 	| Data[8] 	| Data[9] 	| Data[10] 	| Data[11] 	| Data[12] 	| Data[13] 	| Data[14] 	| Data[15] 	| Data[16] 	| Data[17] 	|
    |----------	|---------	|---------	|---------	|---------	|---------	|---------	|---------	|---------	|---------	|----------	|----------	|----------	|----------	|----------	|----------	|----------	|----------	|
    | 17 	| "s" 	| "e" 	| "r" 	| "v" 	| "e" 	| "r" 	| "-" 	| "o" 	| "v" 	| "e" 	| "r" 	| "l" 	| "o" 	| "a" 	| "d" 	| "e" 	| "d" 	|


> You should also print out some local messages.   
+ For the client, please only print out the messages received from server ~~in the same format as the client receives (you could ignore the header parts: msg flag, word length and number guesses, and add line break character at the end of the message)~~ in the following format:
  + Normal game control packet from server (_i.e.,_ packets whose **Msg flag** is zero) should be formated as follows:
    + The first line contains the word information, note that adding a blank space between any tow contiguous letters or "_"
    + The second line contains all previous guesses that are incorrect (a blank space between any tow contiguous letters)
    + Blank line

    For example,
    ```bash
    g _ e e _
    Incorrect Guesses: A B C

    ```
  + Other packets received from server (_e.g.,_ "Game Over", "You Win", "You Lose:("), printing out messages out in the same order they are received (ignoring the **Msg flag**), for example,
    ```bash
    You Win!
    ```
  + "Ready to connect" message:
    ```bash
    Ready to start game? (y/n):
    ```
  + "Letter to guess" message:
    ```bash
    Letter to guess:
    ```
  + "Error Input" message,
    ```bash
    Error! Please guess a letter.
    ```

+ For server, we expect you print out messages about the connection status, for example, "Get connected from client_ip:client_port_number" or "End the connection from client_ip:client_port_number".

**Note that, please do not print out other messages unless you have very strong reason to do so!!! Otherwise you might lose points!!!**

## Grading

This is not a performance-oriented project; we will test the functionality only.
The Rubric will be: 

1. server functionality - 6 points (**lose up to 2 points if failing to follow the message format**)
2. client functionality - 2 points (**lose up to 1 points if failing to follow the message format**)
3. README + Makefile (if using C/C++/Java) - 2 points (**lose up to 6 points, if no Makefile and README**)
4. Extra - 2 points


### How Will TAs Test Your Codes

Unlike the previous project, TAs will test your codes manually for this project. The basic test flow would be as follows. 

+ start [wireshark](https://www.wireshark.org/) capturing all traffic on port 2017
+ start server listening to port number 2017
+ start four or more clients one by one ==> we expect to receive "server-overloaded" messages from all clients expect the first three
+ playing the game
+ start another client when one or more clients finished the game
+ stop all clients and server
+ stop wireshark capturing
+ check all messages printed out by server and clients 
+ check decoded traffic by wireshark 


### Where will TAs Test Your Codes

On a VM which installs Ubuntu 16.04, more details:

+ gcc 5.4.0 for C/C++
+ Python 2.7.12 or Python 3.5.2
+ Java version: 1.8.0 (Oracle Java)

**Note that, if you need TAs to test your codes in an environment other than the above one. Please specify in the README file!!!***

## Implementation Tips

### Implementation Tips 1 --- Which environment should we use to develop?

In general, Linux or UNIX based operating systems are preferable, you are not suggested to use Windows. If you only have Windows machine, you can either use [VirtualBox](https://go.oracle.com/LP=37539?elqCampaignId=49449&src1=ad:pas:go:dg:virt&src2=wwmk160606p00147c0001&SC=sckw=WWMK160606P00147C0001&mkwid=sUMCzyIgp|pcrid|174389027913|pkw|virtualbox|pmt|e|pdv|c|sckw=srch:virtualbox) or [VMWare Player](https://www.vmware.com/products/workstation-player.html) to create a Linux VM. Or you can use Linux VMs from AWS or Google (It seems that both AWS and Google provide some free credits for students).

### Implementation Tips 2 --- Where can we find the APIs I might need to use?

1. [cplusplus.com](http://www.cplusplus.com/) for C/C++
2. [Stack Overflow](https://www.google.com/search?q=stack+overflow&oq=stack+over&aqs=chrome.0.0j69i65j0j69i57j69i60l2.2206j0j1&sourceid=chrome&ie=UTF-8) for C/C++/Java/Python


## Hints

### Multi-Clients Connecting to The Same Server

[Here](./multi-clients) gives you an example of how to support multiple clients connecting to the same server. 
The server side code and client side code are copied from: [geeksforgeeks](http://www.geeksforgeeks.org/socket-programming-in-cc-handling-multiple-clients-on-server-without-multi-threading/) and [stackoverflow](https://stackoverflow.com/questions/27668560/multiple-clients-in-c-socket-programming) respectively. TAs have tested them under both MacOS and Ubuntu 16.04.

The server side code uses [`select`](https://linux.die.net/man/2/select) to support multiple clients connecting to the same server. Note that, this is not the only solution or the best solution, but maybe the simplest solution if you are not familiar with multi-threading. 

Note that, the server side code provided by [stackoverflow](https://stackoverflow.com/questions/27668560/multiple-clients-in-c-socket-programming) does not work for this project, although it actually provides a solution for supporting multiple clients connecting to the same server. However, if you have experience or want to learn about multi-thread programming, you can use multiple threads together with this solution to implement hangman game for this project.

[A tiny introduction to asynchronous IO](http://www.wangafu.net/~nickm/libevent-book/01_intro.html) gives a clearer view of how to achieving multiple clients connecting to the same server.

### MultiThreading 

+ [C++](https://www.youtube.com/playlist?list=PL5jc9xFGsL8E12so1wlMS0r0hTQoJL74M): **at least one of the TAs watched them**. BTW, the TA who watched them thought that the C++11 tutorials created by the same author of these videos are also very helpful, if you are not familiar with C++ and want to learn it.
+ [Python](https://www.tutorialspoint.com/python/python_multithreading.htm)
+ [Java](https://www.tutorialspoint.com/java/java_multithreading.htm)


## Others

You can take a try on others' implementation of hangman game [here](http://www.playhangman.com/).


## Notice

If you have any suggestions for improving this "Not Another Instruction" or you have read/watched some tutorials that are related to this project and very helpful to "make students' life easier" with this project, please feel free to give us feedbacks on piazza or just post an issue for this repository.