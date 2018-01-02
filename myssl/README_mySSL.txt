README.TXT for mySSL
--------------------------------------------

This includes 2.cpp files.
(1)alice2.cpp
(2)bob2.cpp


Description:
(2) .cpp programs has to be run and then (1) has to be run.
Here Alice is the client,and Bob is the server.

Pre-Defined Variables:
- Alice's public key, private key pair, certificate using RSA and openssl
- Bob's public key, private key pair, certificate using RSA and openssl
- certificate for third party like verisign who verifies the client and server certificate, using openssl


Openssl Functions:

AES-128-CBC - to encrypt input data using AES CBC with -e flag
AES-128-CBC - to decrypt the data using AES CBC with -d flag
-generate private key for CA
openssl genrsa -aes256 -out ca.key 2048
-generate certificate for CA
openssl req -new -x509 -days 7300 -key ca.key -sha256 -extensions v3_ca -out ca.crt
-generate private key for server
openssl genrsa -out serverkey.txt 2048
-sign request to CA
openssl req -sha256 -new -key serverkey.txt  -out ser.csr
-sign request using CA, generate certificate
openssl x509 -sha256 -req -in ser.csr -CA ca.crt -CAkey ca.key -CAcreateserial -out ser.crt -days 7300
Similarly do for client

Functions:
----(1,2)----

tokenizer() - Its a parser.

getkey - read the contents from file,(can be private keys, encrypted data)

en_crypt() - encrypt data using other side's public key

de_crypt() - decrypt using the private key generated from public key,private key pair

main() - performs socket programming. /*Pass 1 port number to connect to Bob/Alice*/

@param argv[] - Enter the port number while executing the program. The range is 0-65535. This is passed as argument to main()


To compile: (Needs to have openssl package installed)
	 g++ bob2.cpp -lssl -lcrypto -o bobobj
	 g++ alice2.cpp -lssl -lcrypto -o aliceobj
	
To execute:
	./bobobj 1301
 	./aliceobj 1301

NOTE:

When the connection is force terminated and if the server program and client program is run again, please use different <port_number> while executing to avoid port conflicts. 
The previously assigned port number will take time to get released.

If there's binding error while running it for 1st time, then please use different <port_number> while executing to avoid port conflicts. 
