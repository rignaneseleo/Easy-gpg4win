# Easy-gpg4win
Easy explanation of basic use of gpg4win and GNUpg: https://www.gpg4win.org/ and https://gnupg.org/

### The dictionary
PUBLIC KEY = Key that you have to share with everyone is going to send you data.

PRIVATE KEY = Key that you MUST NOT share with anyone. It allows you to decrypt what is crypted with your public key.

### The logic
Everything crypted with the public key is decryptable ONLY with his corrisponded private key AND VICE VERSA.

When you want to send crypted data you need to crypt your file with the recipient's public key.
The recipient can decrypt the data with his private key.

### Create your own keys
You have first of all generate your public and private keys. 
Use the command:

	gpg --gen-key
            
You have to insert (without spaces is better) the name of your key [key_name], your email and a passphrase (it's a sort of password that you need to insert everytime you use the private key).

### Show your keys
      
If you created correctly your keys before, they should be shown here.
You can see the public keys stored in your pc with:
            
	gpg --list-keys
                
You can see the private keys stored in your pc with:
            
	gpg --list-secret-keys
               
### Show your keygrips

You can see the keygrips codes of all the public and private keys stored in your pc with:

	gpg --with-keygrip --list-secret-keys
	
You can see the keygrip and fingerprint code of a specific key stored in your pc with:

	gpg --fingerprint --fingerprint --with-keygrip [key_name]

You can check a keygrip with:

	gpg-connect-agent
	> havekey [keygrip]

### Import the recipient's public key
                
As I said before, you need to have the public key of your recipient if you want to send him cryptated files.
Ask him the file of the key (we'll see how to export the key later) and import it in your pc with:

	gpg --import [key_file_path]      
            
                
### Export your public key as a file
                                
With the following command, you can create a file contaning your public key to share with everyone is going to send you crypted data:

	gpg -o [file_output_path] --export [key_name]

To delete a public key in your pc, you can use:

	gpg --delete-key [key_name] 
                
### Export your private and public keys as a file

With the following command, you can create a file contaning both your private and your public key to put somewhere safe.
Your passphrase is needed. DO NOT SHARE THIS FILE WITH ANYONE!

	gpg -o [file_output_path] --export-secret-keys [key_name]
          
You should use his file as backup of your private key. You can restore both private and public key with the same import command I wrote before (that command imports only public key if the file contains only a public key and a private and public keys if it has both of them)

To delete a public key in your pc, you can use:

	gpg --delete-secret-key [key_name] 
                               
## Crypt
You will use here only the public key of recipient

	gpg -o [file_output_path] -r [recipient_key_name] -e [file_input_path]
                
## Decrypt
You will use this only to decrypt files crypted with your public key. Your secret key is automatically selected (you don't need to write it also if you have more than one of them). You have to manually write the secret key passphrase. 

	gpg -o [file_output_path] -d [file_input_path]
	   
## gpg-agent.conf
gpg-agent.conf is the file where the settings of the cache are stored.

### Path
It is created in your homedir. You can read your homedir with:

	gpgconf --list-dirs	

you can edit it and write inside:

	verbose
	allow-preset-passphrase
	
Everytime the gpg-agent.conf is modificated, you have to reload the agent.

### Start the agent
You can use

	gpg-agent --daemon --verbose --allow-preset-passphrase
	
or

	gpg-connect-agent /bye
	
In the first case it reports you all the activities it does.

### Turn off the agent
To turn off any agent:
	
	gpgconf --kill gpg-agent

## Save a passphrase in cache
Everytime you decode qith your private key, the passphrase is required. If you don't want to type it manually everytime, you can store it in the agent's cache. You need to configure your gpg-agent.conf before (read above).

	Kill other agents:

		gpgconf --kill gpg-agent

	Start the agent:

		gpg-connect-agent /bye

	Save the passphrase

		gpg-preset-passphrase --passphrase [passphrase] --preset [private_keygrip]
	
OR

	Kill other agents:

		gpgconf --kill gpg-agent
		
	Start the agent with a presetted passphrase:
		
		gpg-connect-agent "preset_passphrase [private_keygrip] -1 [passphrase_in-HEX]" /bye

	(you can use this tool to convert the passphrase in HEX: http://string-functions.com/string-hex.aspx)
	
IMPORTANT: _the passphrase MUST be composed only by letters and numbers (without spaces), otherwise you'll have problems_
