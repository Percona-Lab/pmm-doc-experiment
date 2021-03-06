# MongoDB

## Passing SSL parameters to the mongodb monitoring service

SSL/TLS related parameters are passed to an SSL enabled MongoDB server as
monitoring service parameters along with the `pmm-admin add` command when adding
the MongoDB monitoring service.

Run this command as root or by using the `sudo` command

```
pmm-admin add mongodb -- --mongodb.tls
```

**Supported SSL/TLS Parameters**

`--mongodb.tls`

    Enable a TLS connection with mongo server

`--mongodb.tls-ca`  (string)

    A path to a PEM file that contains the CAs that are trusted for server connections.

    *If provided*: MongoDB servers connecting to should present a certificate signed by one of these CAs.

    *If not provided*: System default CAs are used.

`--mongodb.tls-cert` (string)

    A path to a PEM file that contains the certificate and, optionally, the private key in the PEM format. This should include the whole certificate chain.

    *If provided*: The connection will be opened via TLS to the MongoDB server.

`--mongodb.tls-disable-hostname-validation`

    Do hostname validation for the server connection.

`--mongodb.tls-private-key` (string)

    A path to a PEM file that contains the private key (if not contained in the `mongodb.tls-cert` file).

```
mongod --dbpath=DATABASEDIR --profile 2 --slowms 200 --rateLimit 100
```
