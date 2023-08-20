**Easy boot2root Machine**

As always lets start with the nmap scan..

Lets scan the ip and see the ip ports that are open and which can be used to connect with the server

![Screenshot_2023-08-20_11_53_24](https://github.com/Anirudh-Saxena/Mustacchio-Writeup/assets/73027020/993f42d9-9a86-4f9b-85a9-192739f3fa29)

And the second step lets use `gobuster` and see the dir which are useful to us..

Here is the result of the scan..

![mgobi](https://github.com/Anirudh-Saxena/Mustacchio-Writeup/assets/73027020/5e1e9111-3b40-43de-88f9-193e649ead48)

Lets try to visit the `/custom` and see what this dir has to offer

![custom](https://github.com/Anirudh-Saxena/Mustacchio-Writeup/assets/73027020/0f8ee039-cd4e-45b1-804d-e393fb0ad7da)

Intresting we have `.BAK` file, so basically `.BAK` files store the passwords of the user in the hash format

Lets download this file and see the contents of this file

![bak](https://github.com/Anirudh-Saxena/Mustacchio-Writeup/assets/73027020/2118cf15-7014-42b9-8335-0cb73c5171c2)

Now in order to decode this hash i visited https://crackstation.net/

Now we have the admin password lets try to login with this 


        <targetip>:8765

And login as admin there

Okie so we are redirected to this good looking website

![admin](https://github.com/Anirudh-Saxena/Mustacchio-Writeup/assets/73027020/3ed85d49-0e22-4469-95ed-4c2de2449391)

Lets view the source code of the site and see if we can find any useful information..

The source code has something like to say ` Barry, you can now SSH in using your key!`

So i guess we have to do something on the site so that we can get the `barry's` sshkey such that we can login and get the flag of the user..

After this i used burpsuite to see how the website is actually responding and what is the input the website is taking..

Okie so the website is taking input in the form of `XML` so we have to exploit this such that we can force the website to give the barry's ssh key..


Then i visited this site and copied the script from here : https://owasp.org/www-community/vulnerabilities/XML_External_Entity_%28XXE%29_Processing
and used this code 


    <?xml version="1.0" encoding="UTF-8"?>
    <!DOCTYPE author [<!ENTITY read SYSTEM 'file:///home/barry/.ssh/id_rsa'>]>
    <root>
    <author>&read;</author>
    </root>
![key](https://github.com/Anirudh-Saxena/Mustacchio-Writeup/assets/73027020/c00ae523-bc7b-4045-bbcd-2ec55cd62dd0)



Bravoo now lets use this key and login

First of all lets copy this key and save this key and give him the `chmod 600` permission..

And lets try to crack the key ussing john first lets convert this file using john

        find john

then


     /usr/share/john/ssh2john.py id_rsa > id_rsa.hash

after this

      john <fname> --wordlist=/usr/share/wordlists/rockyou.txt

![johmn](https://github.com/Anirudh-Saxena/Mustacchio-Writeup/assets/73027020/2fe4b0e7-7fec-4ed0-9329-7dcb0666d952)

Now lets connect to the server and get the access of it

![flag1](https://github.com/Anirudh-Saxena/Mustacchio-Writeup/assets/73027020/b6a73eaf-f720-49aa-b4bf-03f4ea75f399)

Lets goo we got the first flag, now lets look at the second question..

For this lets look at the users present in the machine and seeif we  can find anything userful


