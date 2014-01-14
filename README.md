HRIX
====

Human Readable Indicator Exchange (HRIX)
```
Principals: 
Simple Easy to use, reads like normal text and dosen't take much extra thought. 
CRUD capable .. to an extent. 
Generate Event based on new email/message/alert. Update/add/remove with additional replys info. 
Highest TLP (Sharing Classification) prevails. 
Parsed by mail client plugin / parser or messaging plugin ... rips to HRIX db then connects up to threat product of choice or exports IOC/MISP/ARCHER etc.

Syntax Parsing: 
---------
NAME:Indicator (Comments or NAME:Indicator to add context)
---------
Indicators in context/comment parens will be added as their own entities with inversed context to create proper linkage and tracking
- - - 
Example:

From: chris@place.com 
Subject: System Update Phish

We just saw SENDER:john@badness.com (all of them were from SENDINGIP:222.111.001.001) phished us with SUBJECT:"System
Update" .. the attachment was FILENAME:update.exe we haven't had a chance to do any more than that but we saw the
file detected as malicious in VT here LINK:https://www.virustotal.com/en/file/200f56fec7d3b793662ad9481f153f80cc79bc0f76ba999b8f5c24cea1ee9d88/analysis/ 
... you guys Know about it? This is TLP:RED. 

- - -
DB Entry: 

TLP:RED
DESCRIPTION: System Update Phish
TIMESTAMP: 1/10/14 10:36 AM EST
OWNER: chris@place.com 
UUID: 08247082703470923344552222

SENDER: john@badness.com // SENDINGIP: 222.111.001.001
SENDINGIP: 222.111.001.001 // SENDER: john@badness.com
SUBJECT:"System Update"
FILENAME:update.exe 
LINK:https://www.virustotal.com/en/file/200f56fec7d3b793662ad9481f153f80cc79bc0f76ba999b8f5c24cea1ee9d88/analysis/ 
----------

Multiple of a type can be entered as you would in language by pluralizing and comma seperating. 
- - -
Example: 
THREATACTOR:monkeypanda uses C2IPS: 222.222.222.222, 111.111.111.111 also these C2DOMAINS:badness.net, donkey.com 
- - -
DB SNIP:
THREATACTOR: monkeypanda
C2IP: 222.222.222.222
C2IP: 111.111.111.111
C2DOMAIN: badness.net
C2DOMAIN: donkey.com 
-----------

Each email/comment/reply can add new, remove, or propose new indicators as shown in the email thread below. 

?NAME?:Indicator for proposed but uncertain things
!NAME!:Indicator to remove a indicator
NAME:Indicator to add proposed indicator 

Priority goes !NAME! --> NAME --> ?NAME? ... 

- - -
Example:

Is ?C2DOMAIN?:donkey.com related? 

- - - 

DB SNIP
POSSIBLE_C2DOMAIN: donkey.com  

- - -
REPLY: 

No !C2DOMAIN!:donkey.com is used by someone else, but C2DOMAIN:doonkey.com is part of this activity. 

- - -
DB SNIP
C2DOMAIN:doonkey.com 

------------



Full Thread Sample of HRIX: 
 

---- Initial Email / Thread ----

From: john@nationbank.com
Subject:Bad.com Phishing


We have been seeing a phishing attack from SENDER:john@bad.com this morning SUBJECT:"new invoice 32345" (Last numbers change). The file attached was FILENAME:bogus.zip 
(MD5:3333344feadefe444422aaaaa2233333) it contained FILENAME:bogus.exe (MD5:3322244feadefe444422aaaaa2233333)  Some indicators from the malware were C2IP:111.111.111.111 it 
also showed DNS traffic to C2DOMAIN:some.badness.net the MALUA:"lynx 3.24" was used. Inside the phish there was an XMAILER:"verybad.23" and it came from SENDINGIP:32.32.23.23.  
We were a TARGETORG:Nationbank.  This info is TLP:AMBER   


------- EVENT CREATION IN DB ----


TLP:AMBER
DESCRIPTION: Bad.com Phishing
TIMESTAMP: 1/14/14 10:36 AM EST
OWNER: john@nationbank.com
UUID: 082470827034709274074479027494

XMAILER:"verybad.23"
SENDINGIP:32.32.23.23
SENDER:john@bad.com
SUBJECT:"new invoice 32345" // Last numbers change
FILENAME:bogus.zip   // MD5:3333344feadefe444422aaaaa2233333
FILENAME:bogus.exe   // MD5:3322244feadefe444422aaaaa2233333
MD5:3322244feadefe444422aaaaa2233333 // FILENAME:bogus.zip
MD5:3333344feadefe444422aaaaa2233333 // FILENAME:bogus.exe
C2IP:111.111.111.111
C2DOMAIN:some.badness.net
PDNS:other.maybebad.net // C2IP:111.111.111.111 
TARGETORG:Nationbank
LINK:http://www.fireeye.com/blog/technical/2013/11/new-ie-zero-day-found-in-watering-hole-attack.html
LINK:https://www.virustotal.com/en/file/200f56fec7d3b793662ad9481f153f80cc79bc0f76ba999b8f5c24cea1ee9d88/analysis/


----- Email / Thread Reply -----

From: mike@ged.com
Subject: RE: Bad.com Phishing


We also saw it TARGETORG:ged and I found the sample LINK:https://www.virustotal.com/en/file/200f56fec7d3b793662ad9481f153f80cc79bc0f76ba999b8f5c24cea1ee9d88/analysis/ 
on VT which might help, also found this domain in PDNS:other.maybebad.net  (C2IP:111.111.111.111)... .... actually lets make it TLP:RED 

Referenced here as well LINK:http://www.fireeye.com/blog/technical/2013/11/new-ie-zero-day-found-in-watering-hole-attack.html


------- EVENT UPDATE IN DB ----- 

TLP:RED
DESCRIPTION: Bad.com Phishing
TIMESTAMP: 1/14/14 10:36 AM EST
OWNER: john@nationbank.com
UUID: 082470827034709274074479027494

XMAILER:"verybad.23"
SENDINGIP:32.32.23.23
SENDER:john@bad.com
SUBJECT:"new invoice 32345" // Last numbers change
FILENAME:bogus.zip   // MD5:3333344feadefe444422aaaaa2233333
FILENAME:bogus.exe   // MD5:3322244feadefe444422aaaaa2233333
MD5:3322244feadefe444422aaaaa2233333 // FILENAME:bogus.zip
MD5:3333344feadefe444422aaaaa2233333 // FILENAME:bogus.exe
C2IP:111.111.111.111
C2DOMAIN:some.badness.net
TARGETORG:Nationbank
TARGETORG:ged
LINK:http://www.fireeye.com/blog/technical/2013/11/new-ie-zero-day-found-in-watering-hole-attack.html
LINK:https://www.virustotal.com/en/file/200f56fec7d3b793662ad9481f153f80cc79bc0f76ba999b8f5c24cea1ee9d88/analysis/
PDNS:other.maybebad.net // C2IP:111.111.111.111 


------ Email / Thread Reply ----- 


From: ned@awesomesauce.com
Subject: RE: Bad.com Phishing

Hey guys,
    This is 100% THREATACTOR:monkeypanda, I've also seen them use these two SENDERS: donkey@place.com , monkey@place.com. We also know 
    for sure they leveraged this COMPROMISEDSITE: goodsite.com (December 2013).   The following are C2IPS: 111.111.111.110, 111.111.111.114, 
    111.111.111.113, 111.111.111.112 we've seen them use as well. 


------- EVENT UPDATE IN DB ----- 

TLP:RED
DESCRIPTION: Bad.com Phishing
TIMESTAMP: 1/14/14 10:36 AM EST
OWNER: john@nationbank.com
UUID: 082470827034709274074479027494

XMAILER:"verybad.23"
SENDINGIP:32.32.23.23
SENDER:john@bad.com
SUBJECT:"new invoice 32345" // Last numbers change
FILENAME:bogus.zip   // MD5:3333344feadefe444422aaaaa2233333
FILENAME:bogus.exe   // MD5:3322244feadefe444422aaaaa2233333
MD5:3322244feadefe444422aaaaa2233333 // FILENAME:bogus.zip
MD5:3333344feadefe444422aaaaa2233333 // FILENAME:bogus.exe
C2IP:111.111.111.111
C2DOMAIN:some.badness.net
TARGETORG:Nationbank
TARGETORG:ged
LINK:http://www.fireeye.com/blog/technical/2013/11/new-ie-zero-day-found-in-watering-hole-attack.html
LINK:https://www.virustotal.com/en/file/200f56fec7d3b793662ad9481f153f80cc79bc0f76ba999b8f5c24cea1ee9d88/analysis/
PDNS:other.maybebad.net // C2IP:111.111.111.111 
THREATACTOR:monkeypanda
SENDER: donkey@place.com
SENDER: monkey@place.com
COMPROMISEDSITE: goodsite.com // December 2013
C2IP: 111.111.111.110
C2IP: 111.111.111.114
C2IP: 111.111.111.112
C2IP: 111.111.111.113

------ Email / Thread Reply ----- 


From: chris@goggles.com
Subject: RE: Bad.com Phishing

We were a TARGETORG:goggles as well, specifically our TARGETVERTICAL:finance division. Those guys did their homework same sender. They dropped the following malware, thanks for the heads up on the last IP Ned that saved us when it called home!! 
MD5:defe444422aaaaa3333344fea2233333 (C2DOMAIN: goggles.badness.net C2IP:111.111.110 FILENAME:powned.exe DROPPEDFILE:wrecked.exe) 
MD5:daaaaa3333344fea22333efe44442233 (C2DOMAIN: stillpowned.badness.net C2IP:111.111.110 FILENAME:wrecked.exe) 

Anyone seen them use ?FILENAME?:psexe.exe (MD5: defe444422aaaaa3333344fea2233333) ?? I found it on one of our effected users but I have no idea if it's related or something else... weird traffic to ?C2DOMAIN?:goggles.monkey.net using ?UA?:"lynx 993" ... not sure what the hell. 

------- EVENT UPDATE IN DB ----- 

TLP:RED
DESCRIPTION: Bad.com Phishing
TIMESTAMP: 1/14/14 10:36 AM EST
OWNER: john@nationbank.com
UUID: 082470827034709274074479027494

XMAILER:"verybad.23"
SENDINGIP:32.32.23.23
SENDER:john@bad.com
SUBJECT:"new invoice 32345" // Last numbers change
FILENAME:bogus.zip   // MD5:3333344feadefe444422aaaaa2233333
FILENAME:bogus.exe   // MD5:3322244feadefe444422aaaaa2233333
MD5:3322244feadefe444422aaaaa2233333 // FILENAME:bogus.zip
MD5:3333344feadefe444422aaaaa2233333 // FILENAME:bogus.exe
C2IP:111.111.111.111
C2DOMAIN:some.badness.net
TARGETORG:Nationbank
TARGETORG:ged
LINK:http://www.fireeye.com/blog/technical/2013/11/new-ie-zero-day-found-in-watering-hole-attack.html
LINK:https://www.virustotal.com/en/file/200f56fec7d3b793662ad9481f153f80cc79bc0f76ba999b8f5c24cea1ee9d88/analysis/
PDNS:other.maybebad.net // C2IP:111.111.111.111 
THREATACTOR:monkeypanda
SENDER: donkey@place.com
SENDER: monkey@place.com
COMPROMISEDSITE: goodsite.com // December 2013
C2IP: 111.111.111.110
C2IP: 111.111.111.114
C2IP: 111.111.111.112
C2IP: 111.111.111.113
TARGETORG:goggles
TARGETVERTICAL:finance
MD5:defe444422aaaaa3333344fea2233333 // C2DOMAIN: goggles.badness.net C2IP:111.111.110 FILENAME:powned.exe DROPPEDFILE:wrecked.exe
MD5:daaaaa3333344fea22333efe44442233 // C2DOMAIN: stillpowned.badness.net C2IP:111.111.110 FILENAME:wrecked.exe
C2DOMAIN: goggles.badness.net 
FILENAME:powned.exe // MD5:defe444422aaaaa3333344fea2233333 C2DOMAIN: goggles.badness.net C2IP:111.111.110 DROPPEDFILE:wrecked.exe
FILENAME:wrecked.exe // MD5:daaaaa3333344fea22333efe44442233 C2DOMAIN: stillpowned.badness.net C2IP:111.111.110
POSSIBLE_FILENAME: psexe.exe // POSSIBLE_MD5: defe444422aaaaa3333344fea2233333
POSSIBLE_MD5: defe444422aaaaa3333344fea2233333 // POSSIBLE_FILENAME: psexe.exe
POSSIBLE_C2DOMAIN: goggles.monkey.net 
POSSIBLE_UA: "lynx 993"


------ Email / Thread Reply ----- 


From: paul@banktrust.com
Subject: RE: Bad.com Phishing

Naw chris !FILENAME!:psexe.exe isn't related, though it's no good! ... though I've seen them use C2DOMAIN:goggles.monkey.net in a previous attack so that's def. related. UA:"lynx 993" is an awesome indicator, great find! Should help us track these guys in the future. 



------- EVENT UPDATE IN DB ----- 

TLP:RED
DESCRIPTION: Bad.com Phishing
TIMESTAMP: 1/14/14 10:36 AM EST
OWNER: john@nationbank.com
UUID: 082470827034709274074479027494

XMAILER:"verybad.23"
SENDINGIP:32.32.23.23
SENDER:john@bad.com
SUBJECT:"new invoice 32345" // Last numbers change
FILENAME:bogus.zip   // MD5:3333344feadefe444422aaaaa2233333
FILENAME:bogus.exe   // MD5:3322244feadefe444422aaaaa2233333
MD5:3322244feadefe444422aaaaa2233333 // FILENAME:bogus.zip
MD5:3333344feadefe444422aaaaa2233333 // FILENAME:bogus.exe
C2IP:111.111.111.111
C2DOMAIN:some.badness.net
TARGETORG:Nationbank
TARGETORG:ged
LINK:http://www.fireeye.com/blog/technical/2013/11/new-ie-zero-day-found-in-watering-hole-attack.html
LINK:https://www.virustotal.com/en/file/200f56fec7d3b793662ad9481f153f80cc79bc0f76ba999b8f5c24cea1ee9d88/analysis/
PDNS:other.maybebad.net // C2IP:111.111.111.111 
THREATACTOR:monkeypanda
SENDER: donkey@place.com
SENDER: monkey@place.com
COMPROMISEDSITE: goodsite.com // December 2013
C2IP: 111.111.111.110
C2IP: 111.111.111.114
C2IP: 111.111.111.112
C2IP: 111.111.111.113
TARGETORG:goggles
TARGETVERTICAL:finance
MD5:defe444422aaaaa3333344fea2233333 // C2DOMAIN: goggles.badness.net C2IP:111.111.110 FILENAME:powned.exe DROPPEDFILE:wrecked.exe
MD5:daaaaa3333344fea22333efe44442233 // C2DOMAIN: stillpowned.badness.net C2IP:111.111.110 FILENAME:wrecked.exe
C2DOMAIN: goggles.badness.net 
FILENAME:powned.exe // MD5:defe444422aaaaa3333344fea2233333 C2DOMAIN: goggles.badness.net C2IP:111.111.110 DROPPEDFILE:wrecked.exe
FILENAME:wrecked.exe // MD5:daaaaa3333344fea22333efe44442233 C2DOMAIN: stillpowned.badness.net C2IP:111.111.110
C2DOMAIN: goggles.monkey.net 
UA: "lynx 993"



```
