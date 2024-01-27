# OpenSSL Howto

## Two-step Self-signed

```
# Key considerations for algorithm "RSA" ≥ 2048-bit
$ openssl genrsa -out server.key 2048

# Key considerations for algorithm "ECDSA" ≥ secp384r1
# List ECDSA the supported curves (openssl ecparam -list_curves)
$ openssl ecparam -genkey -name secp384r1 -out server.key

$ openssl req -new -x509 -sha256 -key server.key -out server.crt -days 3650
```

## One-step Self-signed

### ECDSA

```
# ECDSA recommendation key ≥ secp384r1
# List ECDSA the supported curves (openssl ecparam -list_curves)
$ openssl req -x509 -nodes -newkey ec:secp384r1 -keyout server.ecdsa.key -out server.ecdsa.crt -days 3650

# openssl req -x509 -nodes -newkey ec:<(openssl ecparam -name secp384r1) -keyout server.ecdsa.key -out server.ecdsa.crt -days 3650

# -pkeyopt ec_paramgen_curve:… / ec:<(openssl ecparam -name …) / -newkey ec:…
$ ln -sf server.ecdsa.key server.key
$ ln -sf server.ecdsa.crt server.crt
```

### RSA

```
# RSA recommendation key ≥ 2048-bit
$ openssl req -x509 -nodes -newkey rsa:2048 -keyout server.rsa.key -out server.rsa.crt -days 3650
$ ln -sf server.rsa.key server.key
$ ln -sf server.rsa.crt server.crt
```
