# Easy-gpg4win
Easy explanation of basic use of gpg4win and GNUpg: https://www.gpg4win.org/ and https://gnupg.org/

## The dictionary
PUBLIC KEY = Key that you have to share with everyone is going to send you data.

PRIVATE KEY = Key that you MUST NOT share with anyone. It allows you to decrypt what is crypted with your public key.

## The logic
Everything crypted with the public key is decryptable ONLY with his corrisponded private key AND VICE VERSA.

When you want to send crypted data you need to crypt your file with the recipient public key and the recipient has to decrypt the data with his private key.

## Create your own keys
You have first of all generate your public and private key. 
Use the command:

            gpg --gen-key
            
You have to insert the Key Name [key_name] (without spaces is better), Email and Passphrase (it's a sort of password that you need to insert everytime you use the private key).

## Show your keys
      
If you created correctly your keys before, they must be shown here.
You can see the public keys stored in your pc with:
            
            gpg --list-keys
                
You can see the private keys stored in your pc with:
            
            gpg --list-secret-keys
                

## Import the recipient public key
                
As I said before, you need to have the public key of your recipient if you want to send him cryptated files.
Ask him the file of the key (we'll see how to export the key later) and import it in your pc with:

            gpg --import [key_file_path]      
            
                
## Export your public key as a file
                                
With the following command, you can create a file contaning your public key to share with everyone is going to send you crypted data:

            gpg -o [file_output_path] --export [key_name]

To delete a public key in your pc, you can use:

            gpg --delete-key [key_name] 
                
## Export your private and public keys as a file

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
You will use this only to decrypt files crypted with your public key. Your secret key is automatically selected (you don't need to write it also if you have more than one of them).

            gpg -o [file_output_path] --batch --passphrase PASSPHRASE -d [file_input_path]
