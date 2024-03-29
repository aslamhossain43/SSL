

Theory: https://www.youtube.com/watch?v=33VYnE7Bzpk&list=PLSNNzog5eyduzyJ8_6Je-tYOgMHvo344c&index=2 
Example: https://www.baeldung.com/x-509-authentication-in-spring-security

==========START==========

alias keytool='/usr/lib/jvm/java-17-openjdk-amd64/bin/keytool'

Generate Self Signed Root Certificate Authority(CA)

Run 'openssl req -x509 -sha256 -days 3650 -newkey rsa:4096 -keyout /home/aslam/Documents/Career/SSL/SSL/src/main/resources/rootCA.key -out /home/aslam/Documents/Career/SSL/SSL/src/main/resources/rootCA.crt'

Use this password in all place 'aslam143'

Email: aslam@revesoft.com

Create Server-side Certificate

Run 'openssl req -new -newkey rsa:2048 -nodes -keyout /home/aslam/Documents/Career/SSL/SSL/src/main/resources/localhost.key -out /home/aslam/Documents/Career/SSL/SSL/src/main/resources/localhost.csr'

Create file 'touch /home/aslam/Documents/Career/SSL/SSL/src/main/resources/localhost.ext && chmod -R 777 /home/aslam/Documents/Career/SSL/SSL/src/main/resources/localhost.ext' it needs when signing the certificate

authorityKeyIdentifier=keyid,issuer
basicConstraints=CA:FALSE
subjectAltName = @alt_names
[alt_names]
DNS.1 = localhost


Sign the server certificate 'localhost.crt' with rootCA.crt and rootCA.key

Run 'openssl x509 -req -CA /home/aslam/Documents/Career/SSL/SSL/src/main/resources/rootCA.crt -CAkey /home/aslam/Documents/Career/SSL/SSL/src/main/resources/rootCA.key -in /home/aslam/Documents/Career/SSL/SSL/src/main/resources/localhost.csr -out /home/aslam/Documents/Career/SSL/SSL/src/main/resources/localhost.crt -days 365 -CAcreateserial -extfile /home/aslam/Documents/Career/SSL/SSL/src/main/resources/localhost.ext'

To view our own certificate 'openssl x509 -in /home/aslam/Documents/Career/SSL/SSL/src/main/resources/localhost.crt -text'

Complete packaging the localhost.crt and localhost.key using PKCS12

Run 'openssl pkcs12 -export -out /home/aslam/Documents/Career/SSL/SSL/src/main/resources/localhost.p12 -name "localhost" -inkey /home/aslam/Documents/Career/SSL/SSL/src/main/resources/localhost.key -in /home/aslam/Documents/Career/SSL/SSL/src/main/resources/localhost.crt'

Create a keystore.jks repository importing the localhost.p12

Run 'keytool -importkeystore -srckeystore /home/aslam/Documents/Career/SSL/SSL/src/main/resources/localhost.p12 -srcstoretype PKCS12 -destkeystore /home/aslam/Documents/Career/SSL/SSL/src/main/resources/keystore.jks -deststoretype JKS'



server:
  port: 8443
  ssl:
    key-store: classpath:keystore.jks
    key-store-password: aslam143
    key-alias: localhost
    key-password: aslam143
    enabled: true


Import the rootCA.crt to browser for Client-Side certificate
Restart browser


==========END==========

The solution of digital signature is digital certificate

Digital certificate verifies the identity of owner and it's public key

ssl certificate vs digital certificate

asymmetric and symmetric key

Symmetric key is used to both encrypt and decrypt information.


