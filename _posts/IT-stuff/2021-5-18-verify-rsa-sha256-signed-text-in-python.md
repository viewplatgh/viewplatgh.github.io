---
layout: post
title: Verify rsa sha256 signed text in Python
tag: it-stuff
---

I created a gist to keep a basic implementation, in Python. Here it is: https://gist.github.com/viewplatgh/cb690159e8ee028cb4c9483cb127e6c3

```python
# packages required but not limited to:
# asn1crypto==0.24.0
# cryptography==2.2.2
# pyOpenSSL==17.5.0
import crypto

# Use the public key file: 'pub_key_filename' to verify
# if 'signed' is a valid one from 'plain_text'
# if it's valid return True, otherwise return False
def verify_signed_text(pub_key_filename, plain_text, signed):
  rsa_public_key = None
  with open(pub_key_filename, 'r') as pub_key_file:
      rsa_public_key = pub_key_file.read()

  pkey = crypto.load_publickey(crypto.FILETYPE_PEM, rsa_public_key)
  x509 = crypto.X509()
  x509.set_pubkey(pkey)

  signed = base64.b64decode(signed) # Skip this if signed text is not encoded

  try:
    crypto.verify(x509, signed,
                  plain_text.encode('utf-8'), 'sha256')
    return True
  except crypto.Error as err:
    return False
  except BaseException as exp:
    return False
```

Asymmetric encryption use a pair of public & private key to do the crypto job. Private key is the one being used to encrypt (sign) data. Public key is the one to verify if the signed data is made from the true owner of the private key. This is the fundamental thing of how SSH and HTTPS work.
