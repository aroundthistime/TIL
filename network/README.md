# Network

### 1. Process of entering a web page
(1) User **types URL** in the browser<br><br>
(2) The browser **requests DNS to find the ip address** corresponding to the domain name.<br><br>
(3) Using the obtained `ip address` and `port` (default is `80` for `HTTP`, `443` for `HTTPS`), **browser establishes `TCP` connection to the server (`3-way handshake` included).**<br><br>
(4) Server receives the request, processes it and sends the requested resource (eg. `HTML`)<br><br>
(5) Browser creates `Render Tree` out of `DOM Tree` and `CSSOM Tree`. Javascript is also fetched and executed to manipulate the tree.<br><br>
(6) Once all resources are loaded and javascript execution is complete, final layout is rendered and user can interact with the displayed webpage.<br><br>