![image](https://user-images.githubusercontent.com/78603128/119040663-43719300-b9e8-11eb-93b9-9084d21a8b85.png)

First , we slap the ip into our browser to check if port 80(common web server) is up and yes it is !

![image](https://user-images.githubusercontent.com/78603128/119142345-fa1d5400-ba78-11eb-97a9-5e0b27c4618e.png)

look like a credential is required

lets try <a href="https://tools.kali.org/web-applications/gobuster">gobuster(CLI)/dirbuster(GUI)</a> to brute force the web directory for hidden files
![image](https://user-images.githubusercontent.com/78603128/119142807-7879f600-ba79-11eb-8cc8-0236aca6441f.png)
(Note : <br/>
dir specify brute force the directory <br/>
-u specify the web browser url<br/>
-w specify the dictionary path for the attack<br/>
-t specify how many thread to be used for the attack<br/> 
-o specify output file name)<br/>

and we found the hidden directory named :

"home.php" 

![image](https://user-images.githubusercontent.com/78603128/119143134-d8709c80-ba79-11eb-85fb-d91fe7fb960f.png)

guess what ? who need credentials when you can access the page without one ?

"validate.php"

![image](https://user-images.githubusercontent.com/78603128/119142989-b37c2980-ba79-11eb-82ae-a0ab52a03f25.png)

