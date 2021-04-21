# HTTP Packet Stealer

# Intro 

Sockets give us shear amounts of power, since in some sense you can control the behaviour of the process on the other end of the socket. With great power comes great responsibility, We’re learning some evil stuff to be aware of them and to find techniques to mitigate vulnerabilities. 
One other aspect that you might have noticed that everything is strictly defined and network elements are dumb, manipulating the packets on the network can let you for example impersonate some PCs, fake some IPs or even disconnect others from WiFi. 
An HTTP Proxy which forwarded clients’ requests to their destination, We're able to read the HTTP request as it was plaintext, but what if that client was logging into their bank account or their facebook account? Would we be able to steal their passwords? The answer is YES, it was possible back then when HTTP was the mainstream internet protocol. Nowadays we have HTTPS where all of the request packets are encrypted. That doesn’t mean that nobody can check out your data, but it narrows down that number so much.
In this project. We’ll steal some HTTP information by making a program that listens to TCP packets then steals the data if it’s an HTTP request and displays that request.

# Objective 
We’ll learn how to use the third and final type of sockets, which is SOCK_RAW, this type of sockets access to everything as the operating system, depending on its address family. 
By the end of this lab, you’ll have a much deeper understanding of how network packets look like and you’ll learn how to parse IP protocol network packets that are in L3 in the TCP/IP model. (note that this is not the OSI model).
You should also have the feeling of the level of accuracy required in code that's related to networks. Since all the parties involved in a networked system are not known to each other, everything needs to be specified in a very detailed manner.

# Requirements
Operating System: Linux
You can find a video tutorial that relates to the project here (the sound is a bit loud, feel free to lower it from the player)
You're asked to implement a Wirehsark-ish packet stealer that will track HTTP requests/replies and print them to console using RAW AF_INET sockets.
Using the sockets as illustrated in the video will allow you to access IP packets. 
   1. You'll need to parse IP packets to extract their data. 
   2. After that if they hold a TCP packet 
   3. you'll check if that data can be decoded as utf-8 characters or not. 
   4. If so, you'll consider this as an HTTP packet (regardless of its validity) 
   5. then print it directly to the console.
________________


Parsing the packets requires that you read the header sections in the relevant IP and TCP RFCs. Be informed that both headers are of variable length and make sure you understand how this happens in order to parse the packets correctly. We're using IPv4 only.
Notes
   * IHL in IP header and Data Offset in TCP header fields need special care.
   * Please don't parse all the packet fields; they're not needed. Only those in the skeleton are needed.
   * Building code based on simple test cases isn't correct. Build code based on knowledge from the RFCs instead.
   * This project requires a fair amount of knowledge with bitwise operators, slicing and binary. Kindly check the resources section.
   
# Checklist

> Your code can parse a byte literal containing an IP 
> Your code has sockets set up and can capture TCP packets ONLY 
> Your code extracts the IHL field correctly from the IP header 
> Your code extracts source and destination IPs from the IP header and extracts the payload 
> Your code extracts TCP source and destination ports 
> Your code extracts TCP data offset 
> Your code extracts TCP payload and prints it 

The parse_application_layer_packet will always return the parsed packet and never None

# Testing your code

To start, you can use netcat + TCP (send normal text) and make sure everything is working.
Then, you can use the simple HTTP server provided by Python to avoid getting huge responses, you can launch that server in a directory that preferably contains a simple "index.html" file like so.
python -m http.server
	You can use curl to make HTTP requests without typing them everytime. It's also very useful when using APIs to test things out. 
To send a GET request using curl
curl -X GET http://127.0.0.1:8000/   # make a GET request to the default python server URI
	And for sure, you should always check that your parsing matches the one done by Wireshark.
You can select a packet -> right click -> copy -> as Hex stream. Then you'll need to remove the Link Layer bytes manually then you can use it normally. You can convert a hex stream to bytes using binascii.unhexlify() then compare the results.
