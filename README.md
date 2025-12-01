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
## Zadanie 2.1
1) Pobieramy dane z serwera:
```bash
curl 127.0.0.1:2001/encrypt
```
3) Szyfrujemy:
```bash
echo -n "jan1009" | openssl enc -aes-256-ecb -K c4494df65ccae7c78e7fc685598babdb15f4074c04a733b5717cb28a11830cfc -nosalt -base64
```
5) Wysyłamy na serwer:
```bash
curl -X POST 127.0.0.1:2001/submit -H "Content-Type: application/json" -d '{"session_id":"ff85df8483615717","encrypted_b64":"QQBz2Ou+C+sWJJFia2JvNpw=="}'
```
## Zadanie 2.2
1) Pobieramy dane z serwera:
```bash
curl 127.0.0.1:2002/decrypt
```
3) Dekodujemy Base64 -> cipher.bin:
```bash
echo -n "RchVdGaD5X0KCouwWqYPjg==" | base64 -d > cipher.bin
```
5) Deszyfrujemy AES-256-ECB:
```bash
openssl enc -aes-256-ecb -d -K 09f1cddcc77a0ce5b99bbb959eac7e14a01642dcb2825e3329b4161750325666 -nosalt -in cipher.bin
```
7) Wysyłamy na serwer:
```bash
curl -X POST 127.0.0.1:2002/submit -H "Content-Type: application/json" -d '{"session_id":"581f3380321a6894","decrypted_word":"stephanni"}'
```
## Zadanie 2.3
1) Pobieramy słowo z serwera:
```bash
curl 127.0.0.1:2003/encrypt
```
3) Hashujemy:
```bash
echo -n "rcheluvu" | openssl enc -camellia-128-ecb -K 4971239ca77045cbfd77e8319a82fd36 -nosalt -base64
```
5) Wysyłamy na serwer:
```bash
curl -X POST 127.0.0.1:2003/submit -H "Content-Type: application/json" -d '{"session_id":"49738e5185debd97","encrypted_b64":"E4nwbMJrqHBJt8+N//YTNQ=="}'
```
## Zadanie 2.4
1) Pobieramy dane z serwera:
```bash
curl 127.0.0.1:2004/decrypt
```
3) Dekodowanie Base64 -> cipher2.bin:
```bash
echo -n "Po8rckj2i+oe7YEKbwn61g==" | base64 -d > cipher2.bin
```
5) Deszyfrowanie CAMELLIA-128-ECB:
```bash
openssl enc -camellia-128-ecb -d -K bb2f5fdb55f36478dc79da603adef9b -nosalt -in cipher2.bin
```
7) Wysyłamy na serwer:
```bash
curl -X POST 127.0.0.1:2004/submit -H "Content-Type: application/json" -d '{"session_id":"ca8b117489f577a8","decrypted_word":"jenn04"}'
```
## Zadanie 2.5
1) Pobieramy dane z serwera:
```bash
curl 127.0.0.1:2005/encrypt
```
3) Generujemy klucz szyfrujący:
```bash
openssl rand -hex 24
```
5) Szyfrujemy z użyciem klucza:
```bash
echo -n "penfloor" | openssl enc -aria-192-ecb -K 0124aac95ea0c6c1792a03af617e474dbdfb7ef840739e41 -nosalt -base64
```
7) Wysyłamy na serwer:
```bash
curl -X POST 127.0.0.1:2005/submit -H "Content-Type: application/json" -d '{"session_id":"e6b5e0ef29ad54aa","encrypted_b64":"QaBpOfZAH11wZ5Nzhg0X7Q==","key_hex":"0124aac95ea0c6c1792a03af617e474dbdfb7ef840739e41"}'
```
## Zadanie 2.6
1) Pobieramy dane z serwera:
```bash
curl 127.0.0.1:2006/encrypt
```
3) Generujemy klucz szyfrujący:
```bash
openssl rand -hex 16
```
5) Generujemy wektor inicjalizacyjny:
```bash
openssl rand -hex 16
```
7) Szyfrujemy:
```bash
echo -n "sexy925" | openssl enc -aes-128-cbc -K f28f5f4a3088e9cf4aed24faf146bc9f -iv 563e336f244407be40a904097e2c2713 -nosalt -base64
```
9) Wysyłamy na serwer:
```bash
curl -X POST 127.0.0.1:2006/submit -H "Content-Type: application/json" -d '{"session_id":"c93fe58e49f84430", "encrypted_b64":"sGSeYxrFO0c6AfmWyDkOFQ=", "key_hex":"f28f5f4a3088e9cf4aed24faf146bc9f", "iv_hex":"563e336f244407be40a904097e2c2713"}'
```
## Zadanie 2.7
1) Pobieramy dane z serwera:
```bash
curl 127.0.0.1:2007/decrypt
```
3) Dekodujemy base64 do pliku:
```bash
echo "<encrypted_b64>" | base64 -d > cipher.bin
```
5) Odszyfrujemy AES-256-CBC:
```bash
openssl enc -aes-256-cbc -d -pbkdf2 -pass pass:<password> -iv <iv_hex> -in cipher.bin -out plain.txt
```
7) Wysyłamy na serwer:
```bash
curl -X POST 127.0.0.1:2007/submit -H "Content-Type: application/json" -d '{"session_id":"<session_id>","decrypted_word":"<cat plain.txt>"}'
```
## Zadanie 2.8
1) Pobieramy dane z serwera:
```bash
curl 127.0.0.1:2008/decrypt
```
3) Dekodujemy base64 do pliku:
```bash
echo "<encrypted_b64>" | base64 -d > cipher.bin
```
5) Odszyfrujemy 3DES-CBC:
```bash
openssl enc -des-ede3-cbc -d -pbkdf2 -pass pass:<password> -iv <iv_hex> -in cipher.bin -out plain.txt
```
7) Wysyłamy na serwer:
```bash
curl -X POST 127.0.0.1:2008/submit -H "Content-Type: application/json" -d '{"session_id":"<session_id>","decrypted_word":"<cat plain.txt>"}'
```
## Zadanie 2.9
1) Pobieramy dane z serwera:
```bash
curl 127.0.0.1:2009/decrypt
```
3) Dekodujemy base64 do pliku:
```bash
echo "MPYYTG5pLXG2qlWLBZonJmsgKBrxDFYVCu6kW/qQmg=" | base64 -d > cipher.bin
```
5) Deszyfrujemy AES-256-ECB:
```bash
openssl enc -aes-256-ecb -d -pbkdf2 -iter 356 -nosalt -pass pass:logankayla -in cipher.bin -out plain.txt
```
7) Wysyłamy na serwer:
```bash
curl -X POST 127.0.0.1:2009/submit-H "Content-Type: application/json"-d '{"session_id":"09ad03f7639eafe8","decrypted_word":"princesspooh"}'
```
# Asymetryczne
# Password cracking
# GPG
# Powtórzenie
# Kolokwium
