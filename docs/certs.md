# Uploading SSL certificate to Azure Application gateway

Azure support pfx format for SSL certificates. The certificates that you might have received from your SSL provides will probably be in the .cert format.

## Certificate conversion
To convert the certificate, first combine the domain certificate and the intermediate certificate to a single file

Your domain cert will look something like this

```
-----BEGIN CERTIFICATE-----
MIIFxTCCBK2gAwIBAgIMKKt3n8G02bLvUx5GMA0GCSqGSIb3DQEBCwUAMEwxCzAJ
BgNVBAYTAkJFMRkwFwYDVQQKExBHbG9iYWxTaWduIG52LXNhMSIwIAYDVQQD...
....
....
PnQbBuHnMqQWOPS1LHZAUf79MWGggNP+g00Y5Aw9jFhVBnv1flren2g=
-----END CERTIFICATE-----
```
Your intermediate certificate will look something like this
```
-----BEGIN CERTIFICATE-----
MIIETTCCAzWgAwIBAgILBAAAAAABRE7wNjEwDQYJKoZIhvcNAQELBQAwVzELMAkG
A1UEBhMCQkUxGTAXBgNVBAoTEEdsb2JhbFNpZ24gbnYtc2ExEDAOBgNVBAsT...
....
....
MTh89N1SyvNTBCVXVmaU6Avu5gMUTu79bZRknl7OedSyps9AsUSoPocZXun4IRZZUw==
-----END CERTIFICATE-----
```
Finally when you combine them, the file will look something like this
```
-----BEGIN CERTIFICATE-----
MIIFxTCCBK2gAwIBAgIMKKt3n8G02bLvUx5GMA0GCSqGSIb3DQEBCwUAMEwxCzAJ
BgNVBAYTAkJFMRkwFwYDVQQKExBHbG9iYWxTaWduIG52LXNhMSIwIAYDVQQD...
....
....
PnQbBuHnMqQWOPS1LHZAUf79MWGggNP+g00Y5Aw9jFhVBnv1flren2g=
-----END CERTIFICATE-----
-----BEGIN CERTIFICATE-----
MIIETTCCAzWgAwIBAgILBAAAAAABRE7wNjEwDQYJKoZIhvcNAQELBQAwVzELMAkG
A1UEBhMCQkUxGTAXBgNVBAoTEEdsb2JhbFNpZ24gbnYtc2ExEDAOBgNVBAsT...
....
....
MTh89N1SyvNTBCVXVmaU6Avu5gMUTu79bZRknl7OedSyps9AsUSoPocZXun4IRZZUw==
-----END CERTIFICATE-----
```
Save it to a cert+intermediate.cert file

Make sure you have your key also ready and let's assume it is named ssl-key.key

Now to convert it, use the following command in linux
````
openssl pkcs12 -export -out certificate.pfx -inkey 'ssl-key.key' -in 'cert+intermediate.cert'
```
This will ask for password
```
Enter Export Password:
Verifying - Enter Export Password:
```
Enter a password - make sure to write it down somewhere because this will be required while uploading it to the application gateway.

You cannot put a blank password because Application gateway will not allow you to upload an SSL without a password.

Once that is done, you'll get a certificate.pfx file.

Source: https://blog.mevi.tech/uploading-ssl-certificate-to-azure-application-gateway/