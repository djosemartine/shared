Create Certificate Authority
openssl req -new -newkey rsa:4096 -days 365 -x509 -subj "/CN=Kafka-Security-CA" -keyout ca-key -out ca-cert -nodes

Kafka Broker Certificate
Create Certificate
keytool -genkey -keystore kafka.server.keystore.jks -validity 365 -storepass password -keypass password -dname "CN=localhost" -storetype pkcs12
keytool -list -v -keystore kafka.server.keystore.jks

Signing Request
keytool -keystore kafka.server.keystore.jks -certreq -file cert-file -storepass password -keypass password

Signin the certificate
openssl x509 -req -CA ca-cert -CAkey ca-key -in cert-file -out cert-signed -days 365 -CAcreateserial -passin pass:password

Print certificate in Verbose Mode
keytool -printcert -v -file cert-signed

Create TrustStore
keytool -keystore kafka.server.truststore.jks -alias CARoot -import -file ca-cert -storepass password -keypass password -noprompt

Import ca-cert in keystore
keytool -keystore kafka.server.keystore.jks -alias CARoot -import -file ca-cert -storepass password -keypass password -noprompt

Import signed certificate
keytool -keystore kafka.server.keystore.jks -import -file cert-signed -storepass password -keypass password -noprompt





