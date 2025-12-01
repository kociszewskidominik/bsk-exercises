# Spis:
[Hashowanie](#hashowanie)


[Symetryczne](#symetryczne)


[Asymetryczny](#asymetryczne)


[Password cracking](#password-cracking)


[GPG](#gpg)


[Powtórzenie](#powtórzenie)


[Kolokwium](#kolokwium)


# Hashowanie
## Zadanie 1.1 - OPENSSL OBLICZANIE MD5
1) Pobieramy z serwera słowo do zahashowania:
```bash
curl 127.0.0.1:10001/hash
```
2) Obliczamy MD5 słowa:
```bash
echo -n "<word>" | openssl md5
```
3) Wysyłamy odpowiedź na serwer:
```bash
curl -X POST 127.0.0.1:10001/submit -H "Content-Type: application/json" -d '{"session_id":"<session_id>","hash_hex":"<hash>"}'
```


## Zadanie 1.2 - OPENSSL OBLICZANIE SHA-256
1) Pobieramy z serwera słowo do zahashowania:
```bash
curl 127.0.0.1:10002/hash
```
2) Obliczamy SHA-256 słowa:
```bash
echo -n "<word>" | openssl sha256
```
3) Wysyłamy odpowiedź na serwer:
```bash
curl -X POST 127.0.0.1:10002/submit -H "Content-Type: application/json" -d '{"session_id":"<session_id>","hash_hex":"<hash>"}'
```


## Zadanie 1.3 - OPENSSL OBLICZANIE SHA-512
1) Pobieramy z serwera słowo do zahashowania:
```bash
curl 127.0.0.1:10003/hash
```
2) Obliczamy SHA-512 słowa:
```bash
echo -n "<word>" | openssl sha512
```
3) Wysyłamy odpowiedź na serwer:
```bash
curl 127.0.0.1:10003/submit -H "Content-Type: application/json" -d '{"session_id":"<session_id>","hash_hex":"<hash>"}'
```


## Zadanie 1.4 Argon2id
1) Pobieramy z serwera słowo do zahashowania:
```bash
curl 127.0.0.1:10004/hash
```
2) Wchodzimy do kontenera:
```bash
docker run -it mazurkatarzyna/openssl-332-ubuntu:latest
```
3) Obliczamy Argon2id słowa:
```bash
openssl kdf -kdfopt pass:patryk1986 -kdfopt salt:NaCl2024 -kdfopt memcost:8192 -kdfopt iter:1 -keylen 24 argon2id
```
4) Wysyłamy odpowiedź na serwer:
```bash
curl -X POST 127.0.0.1:10004/submit/argon2id -H "Content-Type: application/json" -d '{"session_id":"<session_id>","hash_hex":"<hex>"}'
```


## Zadanie 1.5 - BCRYPT
1) Pobieramy z serwera słowo do zahashowania:
```bash
curl 127.0.0.1:10005/hash
```
2) Wchodzimy do kontenera:
```bash
docker run -it --entrypoint /bin/sh mazurkatarzyna/htpasswd:latest
```
3) Obliczamy BCRYPT słowa:
```bash
htpasswd -nbB user <hash>
```
4) Wysyłamy odpowiedź na serwer:
```bash
curl -X POST 127.0.0.1:10005/submit -H "Content-Type: application/json" -d '{"session_id": "<session_id>","hash_hex":"<hash>"}'
```
# Symetryczne
# Asymetryczne
# Password cracking
# GPG
# Powtórzenie
# Kolokwium
