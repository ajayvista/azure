# Secure Sockets Layer(SSL)

**SSL**, or Secure Sockets Layer, is an encryption-based Internet security protocol. It was first developed by Netscape in 1995 for the purpose of ensuring privacy, authentication, and data integrity in Internet communications. **SSL** is the predecessor to the modern TLS encryption used today.

SSL certificates and the SSL protocol are not the same thing. The SSL protocol is deprecated; SSL certificates are used by the SSL and TLS protocols. SSL and TLS certificates are the same.

SSL protocols have been deprecated by the Internet Engineering Task Force (IETF), along with TLS 1.0 and 1.1.


**SSL termination** - SSL termination is a process by which SSL-encrypted data traffic is decrypted (or offloaded). Servers with a secure socket layer (SSL) connection can simultaneously handle many connections or sessions. An SSL connection sends encrypted data between an end-user’s computer and web server by using a certificate for authentication. SSL termination helps speed the decryption process and reduces the processing burden on backend servers. - How it works - https://avinetworks.com/glossary/ssl-termination/

SSL Termination (Offloading) Traffic flows between the local client and application gateway using HTTPS. The traffic is unencrypted by the application gateway and travels between the application gateway and back-end applications in Azure in plaintext. This moves the overhead of decryption from the app server to the application gateway.

X.509 certificate is a digital document that has been encoded and/or digitally signed according to RFC 5280.

ASN.1 - it tells  exactly how  should write your object/certificate,  abstract syntax of information but does not restrict the way the information is encoded. Various ASN.1 encoding rules provide the transfer syntax (a concrete representation) of the data values whose abstract syntax is described in ASN.1.

![enter image description here](https://www.tutorialsteacher.com/Content/images/https/ssl-certificate-format.png)

**PEM** - Privacy Enhanced Mail is a Base64 encoded DER certificate.  PEM certificates are frequently used for web servers as they can easily be translated into readable data using a simple text editor.  Generally when a PEM encoded file is opened in a text editor, it contains very distinct headers and footers.

-----BEGIN CERTIFICATE REQUEST-----
MIIB9TCCAWACAQAwgbgxGTAXBgNVBAoMEFF1b1ZhZGlzIExpbWl0ZWQxHDAaBgNV
BAsME0RvY3VtZW50IERlcGFydG1lbnQxOTA3BgNVBAMMMFdoeSBhcmUgeW91IGRl
Y29kaW5nIG1lPyAgVGhpcyBpcyBvbmx5IGEgdGVzdCEhITERMA8GA1UEBwwISGFt
aWx0b24xETAPBgNVBAgMCFBlbWJyb2tlMQswCQYDVQQGEwJCTTEPMA0GCSqGSIb3
DQEJARYAMIGfMA0GCSqGSIb3DQEBAQUAA4GNADCBiQKBgQCJ9WRanG/fUvcfKiGl
EL4aRLjGt537mZ28UU9/3eiJeJznNSOuNLnF+hmabAu7H0LT4K7EdqfF+XUZW/2j
RKRYcvOUDGF9A7OjW7UfKk1In3+6QDCi7X34RE161jqoaJjrm/T18TOKcgkkhRzE
apQnIDm0Ea/HVzX/PiSOGuertwIDAQABMAsGCSqGSIb3DQEBBQOBgQBzMJdAV4QP
Awel8LzGx5uMOshezF/KfP67wJ93UW+N7zXY6AwPgoLj4Kjw+WtU684JL8Dtr9FX
ozakE+8p06BpxegR4BR3FMHf6p+0jQxUEAkAyb/mVgm66TyghDGC6/YkiKoZptXQ
98TwDIK/39WEB/V607As+KoYazQG8drorw==
-----END CERTIFICATE REQUEST-----

	Above is the example of a CSR (certificate signing request) in PEM format.

**CRT** - is a file extension for a digital certificate file used with a web browser. CRT files are used to verify a secure website's authenticity, distributed by certificate authority (CA) companies such as GlobalSign, VeriSign and Thawte.

**PFX** - Personal Exchange Format, is a PKCS12 file. This contains a variety of cryptographic information, such as certificates, root authority certificates, certificate chains and private keys. It’s cryptographically protected with passwords to keep private keys private and preserve the integrity of the root certificates. The PFX file is also used in various Microsoft products, such as IIS.

**CER** file is used to store X.509 certificate. Normally used for SSL certification to verify and identify web servers security. The file contains information about certificate owner and public key. A CER file can be in binary (ASN.1 DER) or encoded with Base-64 with header and footer included (PEM), Windows will recognize either of these layout.


**PVK** files: Stands for Private Key. Windows uses PVK files to store private keys for code signing in various Microsoft products. PVK is proprietary format.


**.KEY**  The KEY extension is used both for public and private PKCS#8 keys. The keys may be encoded as binary DER or as ASCII PEM.
