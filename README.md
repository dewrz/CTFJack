# CTF: Jack-of-All-Trades

We begin by running an NMAP scan to get an idea of what the network looks like and it comes back with an HTTP Apache server running on port 22 and SSH running on port 80 (the ole' switcha-roo).
<br>
<br>
<img src="https://i.imgur.com/KXQlljd.jpg" title="source: imgur.com" />
<br>
<br>
I tried to pull up the page in a browser, but because the server is using port 22, and didn't let me so I used “curl”. It's evident that this is going to be an easy box, because it already gives us a possible username, an image named "stego" and a base64 string that could be a password. They also give you the recovery page. (I later modified the config file of Firefox to allow uncommon ports) 
<br>
<br>
<img src="https://i.imgur.com/aP1PtNj.jpg" title="source: imgur.com" />
<br>
<br>
I decoded the base64 string and it did indeed share a password. Before I try to login, I'm going to grab the two jpegs to see if there is any hidden information.
<br>
<br>
<img src="https://i.imgur.com/Fzr7Pea.jpg" title="source: imgur.com" />
<br>
<br>
<img src="https://i.imgur.com/73kqKS9.jpg" title="source: imgur.com" />
<br>
<br>
It turns out the password was used for steganography on one of the images that were downloaded. Using Steghide to extract a file revealed that the first two images were a wild goose chase, so I went back to take a look at the assets directory again and pulled the image that was hiding the actual credentials. 
<br>
<br>
<img src="https://i.imgur.com/NpiGJvD.jpg" title="source: imgur.com" />
<br>
<br>
I opened the browser and used recovery.php which naturally displayed an authentication page, and it entered the credentials. After authentication, this message is returned, which leads me to believe that it is vulnerable to command injection. 
<br>
<br>
<img src="https://i.imgur.com/o6E3oVk.jpg" title="source: imgur.com" />
<br>
<br>
<img src="https://i.imgur.com/vBinqmY.jpg" title="source: imgur.com" />
<br>
<br>
<a href="https://imgur.com/fTMHWVB"><img src="https://i.imgur.com/fTMHWVB.jpg" title="source: imgur.com" /></a>
<br>
<br>
I then started a netcat listenter on my attack machine and injected a netcat reverse shell. On the attack machine where the listener was set, I was able to look into the servers home directory to find a password list.
<br>
<br>
<img src="https://i.imgur.com/KnYJzCA.jpg" title="source: imgur.com" />
<br>
<br>
<img src="https://i.imgur.com/53TX3SU.jpg" title="source: imgur.com" />
<br>
<br>
Using Hydra, I ran the password list using "jack" as the login against SSH and then authenticated to the server through port 80. 
<br>
<br>
<img src="https://i.imgur.com/2RfNswz.jpg" title="source: imgur.com" />
<br>
<br>
Looking into jacks directory we see there is no user.txt flag, but a user.jpg image. So, I used secure copy to grab the image and view it on my attack box. 
<br>
<br>
<img src="https://i.imgur.com/212FOj9.jpg" title="source: imgur.com" />
<br>
<br>
<img src="https://i.imgur.com/bD1IzYV.jpg" title="source: imgur.com" />
<br>
<br>
Opening the image on our attack machine revealed the user flag.
<br>
<br>
** I forgot to take an screen shot of the user.jpg flag.
<br>
<br>
Now we need to elevate our privileges to root so we can retrieve the last flag. I proceeded through my usual workflow, checking cronjobs first revealed nothing interesting, but when checking for binaries I may be able to exploit, it looks like we are able to take advantage of the strings binary. Being able to exploit this revealed the root flag. 
<br>
<br>
<img src="https://i.imgur.com/B8w40ou.jpg" title="source: imgur.com" />
<br>
<br>
<img src="https://i.imgur.com/nV5iH9D.jpg" title="source: imgur.com" />
<br>
<br>
I was fooled in the beginning into believing that this was going to be a quick and easy CTF(such is the nature of CTF's), but it was fun and gave a bit of a challenge. 
































