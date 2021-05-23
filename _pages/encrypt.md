---
permalink: /Crypto/
title: "Encrypt"
---
 <style> .indented { padding-left: 10pt; padding-right: 10pt; } </style>
<style> .half {     display: block;
  margin-left: auto;
  margin-right: auto; width: 50%; } </style>
<style>
    figure {
    display: inline-block;
    margin: 20px; /* adjust as needed */}
    figure img {
      vertical-align: top;}
    figure figcaption {
    text-align: center;}
 </style>
## Encryption Utility
<center><img src ="https://github.com/dunhamc13/dunhamc13.github.io/blob/master/teaser.png?raw=true" class="half"></center>  


## Running the applications

### Runtime: .NET 5.0


### Configurations
Command Line Arguments:`

password : user created to decrypt file must be 24 characters

key size(128,192,256) : program supports AES128 AES256 and 3DES192, input just the keysize in bits 

hash size(256, 512) : hash algorithm is SHA256 or SHA512 only put in the bits 

iterations : number of iterations to hash master key 

file to encrypt : in VS proj append ../../../file.ext to place in folder with other files

### Current Description of Implementation
The progam uses a driver that checks command line arguments.  Once arguments are set it will first
enrypt a file to a byte array.  Byte array then has additional header material for decryption added.  For tesint purposes
the utility automatically decrypts that file on a round trip.  Utility removes extra padding at end of decrypt.

### Current Description of future Implementation
3DES needs more modularaization.. needs to be separated out.

Will move the driver to user prompt to allow a user to start with decryption instead of encryption.

## Performance Discussions on PBKDF2
Key derivations are done using PBKDF2 provided in the [.NET 5.0](https://docs.microsoft.com/en-us/dotnet/api/system.security.cryptography.rfc2898derivebytes?view=net-5.0)
 library. Master key derivation was completed to examine creation times.

The six combinations of six cases are:
1. AES128 using SHA256 and SHA512
1. 3DES192 
1. AES256 using SHA256 and SHA512

There was no significance in difference in encryption times.  Therefor the program defaults to AES256 with SHA512 for the best security.


Depending on the use of the program and the user will determine what is the balance between performance and security, 10,000,000 iterations can be completed on an average of 7 seconds.
1,000,000 iterations were completed on an average of 0.75 seconds in the KDF function.  Due to the low number of encryptions being 
performed the table below summarizes a balance of 5,000,000 iterations for the entire file encryption, not just key generation (please note these were ran on a 32 CPU core your performance may vary):

| File               | Size            | Time Encryption | 
| :-----------------:| :-------------: | --------------: | 
| Excel Workbook     |      9 kB       | 3.72 secs        | 
| PNG Image          |     175 kB      | 3.74 secs        | 
| TXT                |     1 KB        | 3.73 secs        |
| ISO                |     2.034GB     | 13.78 secs
 
Key generation iteration is not the bottleneck of the encryption. The size of the file to encrypt is
what creates the largest overhead.  I am personally comfortable with a 3 second wait time to encrypt a single file.

