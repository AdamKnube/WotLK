This repo was built from the awesome work of @Digital-Scriptorium on YouTube.
You can check it out here:

 - https://www.youtube.com/watch?v=Kw_xhyssmTw.

I tried to keep as much of his hard work as possible, including most of this text.
The entire build script that does the heavy lifting is also all his.
Subscribe to his channel if you enjoy this stuff!

So why do this and not just do that? Well...
I found that while following his guide I made a lot of errors during the initial build.
No fault of his, I just have a different environment and also much human error.
So I tried to adapt for both and automate the build process.

This guide assumes you are running Linux with a bash shell and git/docker installed already.
It also assumes that you have docker knowledge of starting/stopping/restarting containers.
If either of these statements is false, then go check out the above mentioned video!

Most lines that are not enclosed in brackets can be directly pasted into the terminal.
Some lines however may require edits before pasting them, so always read the () comments.
Let's get started.

******[AzerothCore 3.3.5 (WotLK) with Playerbots/Junk2Gold/GroupQuests Setup]******

(Some distros locate bash in a different place then my shebang line.)
(Saying that, if the first command fails with '/usr/bin/bash not found.)
(Then just prepend it with 'bash', no quotes or comma.)

(Depending on your hardware this could take some time.)
(My first clean run on a RockPi4 took over 4000 seconds.)
(However later builds now only take around 400-500 seconds.)
(Be patient.)

git clone https://github.com/AdamKnube/WotLK && cd WotLK && ./build

******(Adjusting Your Realmlist SQL Configuration)******

docker ps -a

(Locate mysql container ID and change 06eb70e041dc in the next command to the one reported by the above command.)

docker exec -it 06eb70e041dc mysql -uroot -p      

(Password: password)

(When inputting the following commands, replace 192.168.1.101 with your IP address.)

use acore_auth
DELETE FROM realmlist WHERE id=1;
INSERT INTO realmlist (id, name, address, localAddress, localSubnetMask, port, icon, flag, timezone, allowedSecurityLevel, gamebuild)
VALUES ('1', 'WoWKnube', '192.168.1.101', '127.0.0.1','255.255.255.0','8085', '0', '0', '1', '0', '12340');
exit

******(Adding an Account)******

(Re-start the stack in this order!!!)
(First stop worldserver, then authserver, finally database.)
(Start back up in reverse order, database, authserver, worldserver.)

docker attach ac-worldserver

(If you are running on a RPi or similar SBC I advise you wait here.)
(It will take a while to initialize everything for the first time, be patient.)
(Once you see a steady stream (10 or more) of:
 - Checking BG Queue...
 - Checking LFG Queue...
 - BG Queue check finished
 - LFG Queue check finished)
(Then it is safe to resume inputting the following commands.)
(Replace Noob with your desired account name.)
(Replace booN with your desired account password.)

account create Noob booN
account set addon Noob 2
account set gmlevel Noob 2 -1

***(Do not press Control + C or you will bring the world server down!)***
(Instead press: Control + P followed by: Control + Q)
(That should be it. You can now proceed to load your client and login.)
(Don't forget to change your realmlist.wtf file.)
(GL!/HF!)

