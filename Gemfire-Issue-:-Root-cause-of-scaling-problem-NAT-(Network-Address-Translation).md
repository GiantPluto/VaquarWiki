feud of NAT with tcp_tw_recycle
Now when multiple client machines are behind a NAT, it can pose a serious trouble, since all of them will have same source IP from server's point of view.
Moreover, TCP timestamp values are generated pseudo-randomly, so each device on the network will have a different timestamp value which can lead to some devices behind a shared IP address passing the tcp_tw_recycle test and others failing and being unable to connect. So, at the end it's all random, which somehow happened to be our case too, wherein out of the two machines which were behind the NAT device, requests from either of them C would fail in random fashion. To explain all this is a simple manner, let us consider the following example :
•	Client 1 and 2 are behind a firewall with NAT.
•	Client 1 makes a successful web request to a WebServer A which has tcp_tw_recycle enabled. Note, that since NAT is enabled, for WebServer A, the request seem to come from Source IP of A, instead of either of client.
•	Let's say WebServer actively closes the connection after sending the necessary data. So the socket in question changes out of TIME_WAIT state and takes a note of the last TCP Timestamp it received from NAT device IP(since Client 1's IP is NAT'ed by the NAT device).
•	Now, if Client 2 tries to connect to the WebServer A by sending a SYN packet, and if its TCP timestamp value happens to be smaller than Client 1’s, the Timestamp value is compared to the previously seen timestamp by the WebServer A and on seeing that the new timestamp is smaller than the previous one, the SYN is dropped.
•	Client 2 is unable to communicate with the WebServer until the TIME_WAIT period expires.
How to fix it?
$ sysctl net.ipv4.tcp_tw_recycle
net.ipv4.tcp_tw_recycle = 1  
Modify the value for the configuration in the file to 0. You need to have root access to do that.
$ cat /etc/sysctl.conf | grep tcp_tw_recycle
net.ipv4.tcp_tw_recycle = 0

$ sysctl -p /etc/sysctl.conf



