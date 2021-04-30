# SSL/TLS

>Theory: https://www.youtube.com/watch?v=UbMlPIgzTxc&list=PLSNNzog5eyduzyJ8_6Je-tYOgMHvo344c
>Example: https://www.baeldung.com/x-509-authentication-in-spring-security

==========START==========

>Create '/hello' rest controller in spring boot

>Generate Self Signed Root Certificate Authority(CA)
1. Run 'openssl req -x509 -sha256 -days 3650 -newkey rsa:4096 -keyout rootCA.key -out rootCA.crt'
>Use this password in all place 'changeit'

>Create Server-side Certificate
2. Run 'openssl req -new -newkey rsa:2048 -nodes -keyout localhost.key -out localhost.csr'


>Sign the server certificate 'localhost.crt' with rootCA.crt
3. Run 'touch localhost.sh' with values
>'authorityKeyIdentifier=keyid,issuer
>basicConstraints=CA:FALSE
>subjectAltName = @alt_names
>[alt_names]
>DNS.1 = localhost'
4. Run 'openssl x509 -req -CA rootCA.crt -CAkey rootCA.key -in localhost.csr -out localhost.crt -days 365 -CAcreateserial -extfile localhost.sh'


>Complete packaging the localhost.crt and localhost.key using  PKCS12
5. Run 'openssl pkcs12 -export -out localhost.p12 -name "localhost" -inkey localhost.key -in localhost.crt'

>Create a keystore.jks repository importing the localhost.p12
6. Run 'keytool -importkeystore -srckeystore localhost.p12 -srcstoretype PKCS12 -destkeystore /${PROJECT-RESOURCES}/keystore.jks -deststoretype JKS'


>Import the rootCA.crt to browser for Client-Side certificate


==========END==========

>The solution of digital signature is digital certificate

>Digital certificate verifies the identity of owner and it's public key

>ssl certificate vs digital certificate

>asymmetric and symmetric key

>Symmetric key is used to both encrypt and decrypt information.


