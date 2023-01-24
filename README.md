# dotnet GitPod

## Next Steps

Click the button below to start a new development environment:

[![Open in Gitpod](https://gitpod.io/button/open-in-gitpod.svg)](https://gitpod.io/#https://github.com/mehdihadeli/dotnet-gitpod)


# Cert

certs/localhost.conf:
``` bash
[req]
prompt                  = no
default_bits            = 2048
distinguished_name      = subject
req_extensions          = req_ext
x509_extensions         = x509_ext

[ subject ]
commonName              = localhost

[req_ext]
basicConstraints        = critical, CA:true
subjectAltName          = @alt_names

[x509_ext]
basicConstraints        = critical, CA:true
keyUsage                = critical, keyCertSign, cRLSign, digitalSignature,keyEncipherment
extendedKeyUsage        = critical, serverAuth
subjectAltName          = critical, @alt_names
1.3.6.1.4.1.311.84.1.1  = ASN1:UTF8String:ASP.NET Core HTTPS development certificate # Needed to get it imported by dotnet dev-certs

[alt_names]
DNS.1                   = localhost
```

``` bash
mkdir certs

openssl req -x509 -nodes -days 365 -newkey rsa:2048 -keyout certs/dotnet-devcert.key -out certs/dotnet-devcert.crt -config certs/localhost.conf --passout pass:111111

openssl pkcs12 -export -out certs/dotnet-devcert.pfx -inkey certs/dotnet-devcert.key -in certs/dotnet-devcert.crt --passout pass:111111

dotnet dev-certs https --clean --import certs/dotnet-devcert.pfx -p "111111"
```