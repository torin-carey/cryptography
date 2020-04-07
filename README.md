pyca/cryptography + ENGINE
==========================

ENGINE Keys
-----------

This repository is for showcasing the use of OpenSSL `ENGINE` supplied
`EVP_PKEY` instances in `cryptography`.

All of the changes here are unstable and interfaces are likely to change.

PKCS#11 Support
---------------

PKCS#11 support is provided through the [libp11 library](https://github.com/OpenSC/libp11)
OpenSSL can be configured to use this library by editing or creating the configured config file
(either `/etc/ssl/openssl.cnf` on some systems or pointed to by `OPENSSL_CONF`) to include sections
along the lines of

```
engines = engine_section

[ engine_section ]
pkcs11 = pkcs11_section

[ pkcs11_section ]
dynamic_path = libpkcs11.so
MODULE_PATH = libp11.so # Or any other PKCS11 library
```

This current version adds a new function `get_engine_key(engine_id, key_id)` in the OpenSSL Backend, which
loads the respective private key `key_id` from `engine_id` like so:

```python3
from cryptography.hazmat.backends import openssl
key = openssl.backend.get_engine_key(b"pkcs11", b"pkcs11:token=Token;object=Label?pin-value=1234")
# key can be used for (some) private key operations such as signing
```

There is currently no openssl error handling or checking support of mechanisms.
