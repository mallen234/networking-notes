# What is a Web server

* A piece of software that sits on a computer with the ability to handle HTTP request and responses.
* When a browser sends a HTTP protocol, web server accepts the requests, finds the requested document, performs any extra actions and sends back a response to the browser.

![[./webservers/web_server.png]] [1]


# Handling Requests
### Inbound requests
A web server handles requests by listening for HTTP or HTTPS requests - usually on port 80 and 443 respectively. An inbounding request will be flagged by the server who will then establish a connection and begin receiving the HTTP request data.
### Parsing requests
The web server has to parse the incoming data which is data split into different categories.
#### Start Line
```
1: An HTTP method: GET/PUT/POST/DELETE
2: The request target which is normally a URL
3: The HTTP version, which indicates the version to send back and the form of the rest of the message
```
#### Headers
Headers follow the same structure: a case-insensitive string followed by a colon (':') and a value whose structure depends upon the header. They allow the client to pass additional information and they can be grouped into four categories:

1. <strong><em>General headers</em></strong> - 
2. <strong><em>Request headers</em></strong> - information about the resource to be fetched, or about the client
3. <strong><em>Representation headers</em></strong>- contains information about the body of the resource like the MIME type or encoding/compression info
4. <strong><em>Payload headers</em></strong> - content length and encoding of payload data

![[webservers/http_request_headers3.png]] [2]
#### Body
The final part of a request is the body and not always used. `GET`, `HEAD` and `DELETE` don't normally require one. In `POST` requests, data that is required for updates will be placed in the body. So Bodies can be split into two categories:
1. <strong><em>Single-resource bodies</em></strong> - These are single files split by two headers: Content-Type and Content-length
2. <strong><em>Multiple-resource bodies</em></strong> - each split body holds a bit of information. This is typically associated with HTML forms.

# Handling Responses
#### Status

The start of a HTTP response begins with the status line and is comprised of three aspects

```
1. The HTTP version
2. An HTTP status code - 200
	* 2** - A succesful response
	* 3** - A redirection response
	* 4** - Client error message eg(404 not found)
	* 5** - Server error message eg(502 Bad gateway)
3. A status text - a short human readable message designed to illuminate the response in a way that is easily understandable
```


The final parts of the response similar to requests with slight differences.
#### Headers

```
* Response headers - These contain additional information that isn't held directly in the status.
* Representation headers - These describe the form of the resource being sent as the term would imply. For instance, the 'content type' header displays the original format of the message plus any encoding applied.
```
#### Body
Again this is not required and depending on the HTTP response code you may not require a body. However it is important a lot of the time and generally will come in two forms as before.
1. <strong><em>Single-resource bodies</em></strong> - These are single files split by two headers: Content-Type and Content-length
2. <strong><em>Single-resource bodies</em></strong> - You can also receive a response that is of unknown length and split into encoded pieces called chunks.
3. <strong><em>Multiple-resource bodies</em></strong> - each split body holds a bit of information. This is typically associated with HTML forms.


# What happens when you type a URL in a browser

1. Your browser looks up the domain name using a DNS lookup
2. Your browser initiates a connection with the server using TCP three way handshake (two generals problem)
3. Your browser sends the information in the form a HTTP request, which is then processed in the way detailed above
4. The browser then renders the returned html to display for you.
![[webservers/OSI-Model.png]] [3]
The OSI model is a common representation of networks that is a useful representation of the journey data embarks on. Data works it's way down the layers all the way to the physical cable and then back up again on the recipients end.
# Understanding TCP
TCP stands for transmission control program and represents a method used for transmission of data across the internet. HTTP is an application level process and TCP sits at the transport level.
HTTP requests use TCP as do many other systems. The reason for this system is due to the way the internet protocol(IP) works. 
### Unreliable IP
When someone sends a packet of data along the internet, IP cannot guarantee a number of things.
1. It can't guarantee the data arrives where you intended it to go. IP is known as a best effort network, so it will try it's best to reach the destination. However, there is a chance your data gets lost along the way for any number of reasons. IP doesn't and there are no processes to resolve this.
2. It also cannot guarantee that your data arrives in the order it was sent. Data is split into chunks and the IP sends them separately, not caring about the final order.
### TCP solution
TCP is the solution to sending data over an unreliable network. IP alone is not enough to ensure we transfer data so TCP has a method to ensure this.
                                    ![[webservers/syn-ack 1.png | center]] [4]

1. It initiates a connection between the two parties using a three way handshake. This uses the TCP three way handshake. On initial request the sender assigns a random number to SYN which the recipient copies when sending the SYN ACK back. The recipient then assigns a random value to ACK which the original sender uses in the final ACK.
2. TCP packages up the data that is ready to be sent, dividing data into chunks with a TCP header.
3. TCP sends the data over IP and appends each header to a list at the same time.
4. The recipient will send back acknowledgements of receipt which will contain the TCP headers
5. As the acknowledgements are streamed back to the sender they remove each corresponding TCP header from their list. 
6. If there are any TCP headers left in the list after a certain specified time, these chunks of data will be sent over again, restarting the process from bullet 3.
7. Once all data is received it is sorted into order by TCP header, which is numbered.

##### References
1. Wikipedia<em> - https://en.wikipedia.org/wiki/Web_server - date accessed 18/10/23 - </em>
2. Mozilla<em> - https://developer.mozilla.org/en-US/docs/Web/HTTP/Messages - date accessed 18/10/23</em>
3. EDUCBA<em> - https://www.educba.com/what-is-osi-model/ - date accessed 18/10/23</em>
4. Techopedia<em> - https://www.techopedia.com/definition/10339/three-way-handshake - date accessed 21/10/23 </em>
