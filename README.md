# Spis:
[Hashowanie](#hashowanie)


[Symetryczne](#symetryczne)


[Asymetryczny](#asymetryczne)


[Password cracking](#password-cracking)


[GPG](#gpg)


[Powtórzenie](#powtórzenie)


[Kolokwium](#kolokwium)


# Hashowanie
### Zadanie 1.1 - OPENSSL OBLICZANIE MD5
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
### Zadanie 1.2 - OPENSSL OBLICZANIE SHA-256
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
### Zadanie 1.3
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
### Zadanie 1.4
### Zadanie 1.5
# Symetryczne
# Asymetryczne
# Password cracking
# GPG
# Powtórzenie
# Kolokwium
