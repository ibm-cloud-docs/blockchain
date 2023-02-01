---

copyright:
  years: 2014, 2023
lastupdated: "2023-01-30"

keywords: code signing, signed images

subcollection: blockchain

---


{{site.data.keyword.attribute-definition-list}}




# Signed images
{: #signed-images}

{{site.data.keyword.IBM_notm}} uses code signing to apply a certificate-based digital signature to files to  verify the author's identity ({{site.data.keyword.IBM_notm}}), and to ensure the contents have not been tampered with or corrupted between the time it was signed by the {{site.data.keyword.IBM_notm}} and received by the customer. The process employs the use of a cryptographic hash to validate software authenticity and integrity, for example, if the hash used to sign the image matches the hash on a downloaded image, the code integrity is intact.
{: shortdesc}

Any validation operation provided in this feature is optional. There is no operational requirement for the customer to validate digital signatures before installing or upgrading.
{: note}

A zip file is included with each new release. This file, that matches the pattern `ibp-<release>.signatures.zip` contains:

- One cryptographic hash file for each release file to be checked. For example, a file named `ibp-<release>-manager-usbiso.iso.sig` would contain the cryptographic hash for the corresponding release file `ibp-<release>-manager-usbiso.iso`.
- Materials for validation:
    - public_key.pem
    - certificate.pem
    - chain.pem


Unzip `ibp-<release>.signatures.zip`

For any file (<FILE>) to be validated against its cryptographic hash, run the command:

```
openssl dgst -sha256 -verify public_key.pem -signature <FILE>.sig <FILE>
```
{: codeblock}

This is a pass/fail operation.  A failure status indicates that the file is not trustworthy and customer support should be contacted.

Additionally, the customer can optionally validate that the public key is present in the certificate and the certificate is still valid. The following command provides a guarantee by the Public Certificate Authority (Digicert), that the private-public keypair that was used to generate the signatures is actually owned by {{site.data.keyword.IBM_notm}}.


```
# Show the certificate details; particularly:
# * It is signed by IBM and the root CA
# * Its Modulus and Exponent

openssl x509 -text -in certificate.pem -noout
# Sample output:
#.  Issuer:  … <CN=DigiCert SHA2 Assured ID Code Signing CA> …
#.       :
#.  Subject: … <CN=International Business Machines Corporation> …
#.       :
#   Modulus:
#        00:e2:45:27:25:e9:a3:1f:c2:37:27:ac:4c:89:86:
#        ae:32:d5:2a:84:69:3b:01:cb:54:34:b0:b3:1b:6d: .......
#.       :
#  Exponent: 65537 (0x10001)



# Show the public key details:
#
openssl rsa -noout -text -inform PEM -in public_key.pem -pubin   
# Sample output:
#.  :
#   Modulus:
#        00:e2:45:27:25:e9:a3:1f:c2:37:27:ac:4c:89:86:
#        ae:32:d5:2a:84:69:3b:01:cb:54:34:b0:b3:1b:6d: .......
#.       :
#  Exponent: 65537 (0x10001)
Compare the above exponent/modulus data outputs of the public key and the certificate to confirm that the public key is indeed the one within the certificate.
Can also check the IBM public certificate validity:

# Check if the cert is still valid:
openssl ocsp -no_nonce -issuer chain.pem \
    -cert certificate.pem -VAfile chain.pem \
    -text -url http://ocsp.digicert.com -respout ocsptest     
```
{: codeblock}

If the certificate is valid, the output looks similar to:
```
Response verify OK
```

This output goes to `stderr`. The command status return value does not indicate validity.
{: important}
