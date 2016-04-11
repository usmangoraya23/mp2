# mp2 (webspy Program)
After writing webspy, you should answer the following questions in a README
le submitted with your assignment:
1. Does using HTTPS obscure the URL being requested? If so, why?
2. Does using HTTPS prevent hackers from knowing which web site a user
is browsing?
Answer:
1. The HTTPS protocol is equivalent to using HTTP over an SSL or TLS connection (over TCP).
Thus, first a TCP connection (on port 443) is opened to the server. This is usually enough to reveal the server's host name (i.e. www.mysite.com in your case) to the attacker. The IP address is directly observed, and:
We usually did an unencrypted DNS query before,
many HTTPS servers serve only one domain per IP address,
The server's certificate is sent in plain, and contains the server name (between multiple ones, maybe),
in newer TLS versions, there is the server name indication, by which the client indicates to the server which host name is wished, so the server can present the right certificate, if it has multiple ones. (This is done to be able to go away from 2.)
Then a TLS handshake takes place. This includes negotiation of a cipher suite and then a key exchange. Assuming at least one of your browser and the server didn't include the NONE cipher in the accepted suites, everything following the key exchange is encrypted.
And assuming there is no successful man-in-the-middle attack (i.e. an attacker which does intercept the connection, and presents a forged server certificate which your browser accepts), the key exchange is secure and no eavesdropper can decrypt anything which is then sent between you and the server, and also no attacker can change any part of the content without this being noticed. This includes the URL and any other part of the HTTP request, as well as the response from the server.
Of course, as D.W. mentions, the length of both request (which contains not much more variable data than the URL, maybe some Cookies) and response can be seen from the encrypted data stream. This might subvert the secrecy, specially if there are only a small number of different resources on the server. Also any follow-up resource requests.
Our password in the URL (or any other part of the request) should still be secure, though - at most its length can be known.
2. I am obviously a huge proponent for securing your website, and while important, HTTPS is not the key. It’s definitely a critical piece of the overarching security wheel associated with website security, but it’s not going to stop websites from getting hacked,  the distribution of malware or keep website owners safe.
It will continue to be a critical piece to ensuring information in transit is protected from attackers, but that’s the extent of the secureness it’ll offer your website. We do have to start talking about encryption at rest, in addition to encryption in transit. If we look at the number of data breaches, HTTPS did little to stop the exfiltration of data.
Additionally, remember that HTTPS will have other detrimental impacts to both security and your business as a whole. One of the biggest things I don’t see anyone discussing is the fact that HTTPS not only blocks the bad guys, but the good guys as well. This means that most of the modern Intrusion Detection / Prevention Systems are also blocked from analyzing the incoming data, it’s encrypted. This is not a reason not to employe HTTPS, but it is a point that you must take into consideration.
I have no doubt that HTTPS will continue to grow in popularity, and I’m ok with that. What I take exception to though is when technology professionals say that one of the driving factors for HTTPS is it’ll secure your website, because it won’t.
