### Communication and Networking
Computers do computation, storage and communicate. So this is the third very important and fundamental aspect of software engineering. The magic of computers begin when they start communicating with each other. This is the origin of internet and the whole big revolution. When we say computers, it also means devices or even applications within computers. 

Fundamentally computers communicate via ports. When a computer is available on network then it is provided with address and a port. This port is assigned to do certain types of communications. E.g. a computer or a program might *listen* on certain port. When it listens then it expects communication to happen in certain *protocol*. Protocol in real life can be compared with mechanism understood by two parties. e.g. when you go and drop a letter in postbox then the postman understands the To and From address in certain format such as pin code and then postman delivers the message (letter) to the destination address. If the same communication is via courier the protocol is different. Or if it is via telegram. 

In computer world most popular protocol is HTTP. There are other protocols such TCP/IP, FTP and now recently you would hear about websockets. 

##### Common Aspects of communication
Communications require a **connection** , **listener**, message to be delivered, addresses (to and from), **a protocol** and most importantly authentication. There after there may or may not be a *response* or acknowledgement. So every protocol will some, all or more of these aspects in it. 


##### HTTP 
Every protocol requires listening (accepting), processing and responding. It requires a connection. It is started with http:// or https:// in the request address. http:// or https:// is then followed with an address (called uri) and the message. Message has certain parts like header and body. Header contains certain information and body is the main message. In http protocol, after the request is sent, the receiver sends a response with a status (200 for success). 

HTTP has several methods: GET, POST, PUT and many more. The receiver declares what kind of messages it will accept. 

##### Difference between HTTP and HTTPS. How does HTTPS provides additional security: 
In HTTPS, how does TLS/SSL encrypt HTTP requests and responses?
TLS uses a technology called public key encryption: there are two keys, a public key and a private key, and the public key is shared with client devices via the server's SSL certificate. When a client opens a connection with a server, the two devices use the public and private key to agree on new keys, called session keys, to encrypt further communications between them.

All HTTP requests and responses are then encrypted with these session keys (this takes place at layer 6 in the OSI model), so that anyone who intercepts communications can only see a random string of characters, not the plaintext.

For more on how encryption and keys work, see What is encryption?
[link for more information](https://www.cloudflare.com/learning/ssl/why-is-http-not-secure/#:~:text=HTTPS%20is%20HTTP%20with%20encryption,normal%20HTTP%20requests%20and%20responses.&text=A%20website%20that%20uses%20HTTP,uses%20HTTPS%20has%20https%3A%2F%2F.)

##### WebSockets

##### APIs

