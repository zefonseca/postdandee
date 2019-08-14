

# postdandee 

Postfix log analyzer, dictionary attack shield

Copyright (C) 2005-2019 Jose Fonseca (https://zefonseca.com/)

This program is free software; you can redistribute it and/or
modify it under the terms of the GNU General Public License
as published by the Free Software Foundation; either version 2
of the License, or (at your option) any later version.

This program is distributed in the hope that it will be useful,
but WITHOUT ANY WARRANTY; without even the implied warranty of
MERCHANTABILITY or FITNESS FOR A PARTICULAR PURPOSE.  See the
GNU General Public License for more details.

You should have received a copy of the GNU General Public License
along with this program; if not, write to the Free Software
Foundation, Inc., 51 Franklin Street, Fifth Floor, Boston, MA  02110-1301, USA.

Version 0.0.3 2006-01-01


### About

The SPAM business model is based on very low conversion rates applied to a very 
high number of successfully delivered messages ("high inbox rate"). 

Sysadmins are encouraged to help reduce the number of delivered messages in
every possible way. 

This script effectively reduced my Postfix server loads and sent spammers a clear 
message that dictionary attacks will not be tolerated here. 

A dictionary attack is a very common method of finding valid conduits to 
deliver SPAM.

The spammer will open one or many connections to your SMTP mail exchanger and
will keep trying names@yourdomain until it matches a valid recipient. When
they find a valid email address that your mail exchanger will accept locally 
they mark your server as 'spam good' for that particular email recipient. Once
marked as such the victim's email address will forever receive SPAM through your
MX.

Needless to say dictionary attacks eat up valuable bandwidth, system resources,
and provide spammers with a functioning mail exchanger for their junk.

By default postdandee will operate at the network level. When it finds a rejected
recipient on the maillog it will send the remote host to 'jail' for a $JAILTIME 
number of seconds. 

Barring connections from the remote host at the network level will make TCP 
connections impossible so SPAM traffic consumes minimum bandwidth.

Exchangers caught by postdandee will also not reach your Spamassassin or other
SPAM filters so your valuable CPU load is preserved.

The 'postdandee' daemon script periodically parses the `tail -n $NUMROWS` 
of $LOGFILE and checks for rejected messages. 

It will block the rejected MX for $JAILTIME minutes through $BLOCKHOSTCOMMAND

It was meant for Postfix only and I have no plans to expand it for Exim or
Sendmail. postdandee is GPL'd so you may adapt the idea for any maillog format.


### Installation

postdandee requires no additional libraries or configuration files. copy it
to your executable bin or an appropriate location

E.g:

    cp postdandee /usr/bin
    chown root /usr/bin/postdandee
    chgrp root /usr/bin/postdandee
    chmod +x /usr/bin/postdandee


We reccommend running postdandee as a daemon right from system bootup by 
adding it to /etc/rc.local or similar startup script.


### Post-Installation

Open the postdandee script on your favorite text editor and set the correct 
values for your system under the "MAIN CONFIGURATION" section of the script.


### Security

postdandee does not listen on network interfaces, does not accept user input 
and is written in Perl. There should not be any security holes.

DISCLAIMER: no warranties implied. See licence agreement.


### Usage

For a test run use the -d switch (debug).       

Usage: 

    ./postdandee
    
or ...
  
    ./postdandee -d 1  
    
...for a test run. 

With the -d 1 option postdandee will not detach and will run verbose mode.


### Tested?

  On: 
				- linux, Fedora Core 3
				- linux, Fedora Core 4

Should work on any UNIX-like environment.


### Requirements

- Perl v. 5.x.x
- Default route blocking requires linux's route reject feature. 
  (Will require any program you set $BLOCKHOSTCOMMAND & $RELEASEHOSTCOMMAND to.)
- Setting a high value for $NUMROWS may increase RAM usage.
- Setting a very low $POLLINGTIMEOUT may increase CPU load unnecessarily.


### Links

Spamassassin (http://spamassassin.apache.org/) 

Postfix SMTP mail exchanger (http://www.postfix.org/)

Development supported by https://www.MercadoViagens.com/

### Author

Jose Fonseca (https://zefonseca.com/)

