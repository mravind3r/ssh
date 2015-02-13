how to connect to ssh ??

1) username@hostname  eg: a-rmandal@chsxedwhdc002

2) ssh chsxedwhdc002  , this will assume the username is the one that is used to login to the os

3) ssh chsxedwhdc002  -l a-rmandal


4) first time you log in , it will ask you to add the key to the known_hosts folder.
If the keys changes , delete the key from known_hosts and try to connect again.


5) It's common to use a cryptographic key rather than a username and password to access an SSH server. Doing so is much more secure and it can be more convenient. Key-based access requires a cryptographic key pair made up of a public key and a private key. The server will have the public key and you'll have the private key. You can generate a different key for each computer you connect from, or copy your private key to each computer you use. There are benefits and drawbacks for each choice.

If you use a separate key for each computer, there's a little bit more setup and management overhead since you'll need to generate a bunch of keys and transfer them all to the server, but if one key is compromised, you can easily remove it from the server and still have access from your other computers. If you opt to copy one private key to each computer you use, there's a little bit less setup, but if the key is compromised, you need to act quickly to generate a new key and delete the old one. How you choose to manage your keys is up to you.


6) generating a key pair . We will send the public key to the ssh server and then use private key to connect to it.

a)ssh-keygen -t rsa
accept the defaults

b)ls .ssh/
id_rsa		id_rsa.pub	known_hosts

c) now to copy the public key to the server
For linux : ssh-copy-id a-rmandal chsxedwhdc002     ( ssh-copy-id username host )
For MAC : scp .ssh/id_rsa.pub a-rmandal@chsxedwhdc002:   ( scp .ssh/id_rsa.pub username@hostname: )

d) now ssh to the server and look for the file id_rsa.pub
copy this key into the .ssh/authorized_keys file

cat id_rsa.pub >> .ssh/authorized_keys


7) now connect to the server using our private key

ssh -i .ssh/id_rsa a-rmandal@chsxedwhdc002

-- should connect without asking for password

8) another way to connect without the long command is via configuration file
-- assuming u copied the public key over to the server
a) create .ssh/config  file
in that add the following

Host prod-dev-node
User a-rmandal
HostName chsxedwhdc002
IdentityFile ~/.ssh/id_rsa

b) from command prompt --> ssh prod-dev-node
-- should connect without asking for password 


 you can add many entries in the config file provided the public keys are copied onto the right servers
eg:
Host qa
User a-rmandal
HostName chsxedwhdc004
IdentityFile ~/.ssh/id_rsa


** the above tab seperated entries  should be on a seperate line each

