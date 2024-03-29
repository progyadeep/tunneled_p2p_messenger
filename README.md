# Tunneled P2P Messenger
A secure, in-browser messaging application that facilitataes communication via an SSH tunneling service and doesn't store your chats.

# How this works
Both participants need to run this program on their computers.<br/><br/>
A node server is locally set up at a chosen port (default=3001) which is going to serve as the incoming message server. This local server is exposed via a secure <b>ssh tunnel</b>. A service called <a href="https://localhost.run">localhost.run</a> is used for this purpose; however, you can use any other service of your choice (like <a href="https://serveo.net">serveo.net</a>, <a href="https://ngrok.com">ngrock</a>, etc) by modifying the code in `host.js` accordingly.<br/><br/>
On receiving messages, the server temporarily stores them in a variable and returns its value (i.e., all unread messages) when the client sends an HTTP request at `/fetch`.<br/><br/>
The client can interact with the program through a simple in-browser UI (HTML webpage). Every 0.5s, a function asynchronously checks for available unread messages and puts them on the HTML page.<br/><br/>
When the client wants to send a message to their peer, an HTTP POST request is sent at the `/` of the peer's exposed node server which then receives the message and stores it, until requested by the peer's client UI, in a variable.

# Prerequisites
1. <a href="https://nodejs.org">Node.js</a>.
2. A free TCP port (default 3001) that <b>Node.js</b> is allowed to access.
3. Firewall access given to Node.js and SSH.
4. <b>If you're on Windows</b>, make sure that `ssh` is available as a shell command. SSH is a default shell command on Linux and MacOS but you can set it up in your Windows CMD by installing third party software like <a href="https://putty.org/">Putty</a> or <a href="https://git-scm.com/downloads">Git Desktop</a>.

# Nodejs dependencies
As defined under ['dependencies'] in <a href="https://github.com/progyadeep/tunneled_p2p_messenger/blob/master/package.json">package.json</a>

# How to run it
Start the program by executing the `host.js` file. After navigating to the root directory of this program in the command line, execute:  

    node host.js
    
This will start the local server and set up the SSH tunnel (ssh tunnel will be set up as a subprocess). When these steps are completed, the program will automatically open the web client.<br/><br/>

The webpage will ask for a peer ID, which is the user address of your peer. After entering the Peer ID, you'll see a chat page and you can start exchanging text messages.

The user address is basically the URL that your local server is publicly accessible through. When you start the SSH tunnel using something like <b>localhost.run</b> you'll see that URL returned in the terminal. To have a consistent address for your peers, you can use a domain or subdomain that points to the current tunneled URL of your server (you'll have to update the DNS records of your domain every time you start the tunnel because that URL is randomly generated every time).

I've used <b>localhost.run</b> as the service to demonstrate the working of this code. You can directly modify the statement according to your service of choice.

# Downtime issues
Since the working of this program relies on an external application - like <b>localhost.run</b> or <b>serveo.net</b> - for the SSH tunnel, issues in the tunnelling app's server will cause this program to malfunction as well.
