# OpenSSL

> Software library for applications that secure communications over computer networks, it contains an open-source implementation of SSL and TLS

# PEM (Privacy Enhanced Mail) 

> An encoding of 64 ASCII characters to send binary files (keys and certs) via mail. it contains alphanumerals, `+` and `=`

```bash
openssl list -cipher-commands

openssl prime 3301
openssl prime -hex CE5

openssl dgst -sha256 -out hash.txt hashme.txt

# cbc, ecb, cfb, ctr, ecn, ofb
# aes-128, aes-192, aes-256
# des, des3
# bf is blowfish
# enc produces an unreadable, unshareable sequence of binary
openssl enc -aes-256-cbc -in encryptme.txt -out encrypted.bin
# encodes result in base64
openssl enc -aes-256-cbc [-salt] -in encryptme.txt -out encrypted.b64 -base64
openssl enc -d [-salt] -aes-256-cbc -in encrypted.bin -out decrypted.txt [-pass pass:azerty] [-base64]
openssl enc -d [-salt] -aes-256-cbc -in encrypted.bin -out decrypted.txt [-pass file:pass.txt] [-base64]

openssl genrsa [-des3] -out private.pem 1024

# Attributes of the key in hex
# Public{PublicExponent: e, Modulus: n}
# Private{PrivateExponent: d, Modulus: n}
# Prime1 p, Prime2 q, Exponent1 d mod (p-1), Exponent2 d mod (q-1), Coefficient q^-1 mod p
openssl rsa -in private.pem -text
# Don't show key in PEM format
openssl rsa -in private.pem -text -noout
# Secure encrypted key with des
openssl rsa -in private.pem -des3 -out private3des.pem
# Export public key
openssl rsa -in private.pem -out public.pem -pubout

# Encrypt with public key
openssl rsautl -encrypt -inkey public.pem -pubin -in encryptme.txt -out encrypted.bin
# Decrypt with private key
openssl rsautl -decrypt -inkey private.pem -in encrypted.bin -out decrypted.txt
# Sign a message
openssl rsautl -sign -in signme.txt -inkey private.pem -out signed.bin
# Verify a message
openssl rsautl -verify -in signed.bin -pubin -inkey public.pem -out hashfile.txt

# Auto sign a cert
# perform Certificat Signing Request (CSR)
openssl req -new -newkey rsa:1024 -keyout cle_serveur.pem -out csr_serveur.pem -nodes
# print req text
openssl req -in csr_serveur.pem -text -noout
# sign the created CSR with the created key
openssl x509 -req -in csr_serveur.pem -out cert_serveur.pem -signkey cle_serveur.pem -days 365
# print cert
openssl x509 -in cert_serveur.pem –text -noout

# CA
#Ubuntu : /usr/lib/ssl/misc/CA.sh
#Fedora : /etc/pki/tls/misc/CA.sh
# Generate files
./CA –newca
# generate key and request
/usr/lib/ssl/misc/CA -newreq
# sign cert
usr/lib/ssl/misc/CA -sign
# generated auto signed CA
openssl req -new -x509 -newkey rsa:1024 -keyout ca_key.pem -out ca_cert.pem -days 365
# fill up form
# generate CSR
openssl req -new -newkey rsa:1024 -keyout cle_serveur.pem -out csr_serveur.pem -days 90 -nodes
# sign the server cert with the created CA
openssl x509 -req -in csr_serveur.pem -out cert_serveur.pem -CA ca_cert.pem -CAkey ca_key.pem -CAcreateserial -CAserial ca.srl
# Verify cert validity
openssl verifiy -CAfile ca_cert.pem cert_serveur.pem
```