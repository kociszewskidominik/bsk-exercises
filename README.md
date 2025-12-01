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

## Zadanie 3.1 - Generowanie kluczy RSA
1) Generujemy parę kluczy RSA:
```bash
openssl genpkey -algorithm RSA -out private_key.pem -pkeyopt rsa_keygen_bits:2048
openssl rsa -pubout -in private_key.pem -out public_key.pem
```
2) Wysłanie kluczy do serwera:
```bash
curl -X POST 127.0.0.1:3001/upload/public -H "Content-Type: multipart/form-data" -F "file=@public_key.pem"
curl -X POST 127.0.0.1:3001/upload/private -H "Content-Type: multipart/form-data" -F "file=@private_key.pem"
```

## Zadanie 3.2 - Generowanie kluczy EC
1) Generowanie pary kluczy EC:
```bash
openssl genpkey -algorithm EC -pkeyopt ec_paramgen_curve:prime256v1 -out private_key.pem
openssl ec -in private_key.pem -pubout -out public_key.pem
```
2) Wysłanie kluczy do serwera:
```bash
curl -X POST 127.0.0.1:3002/upload/ec/public -H "Content-Type: multipart/form-data" -F "file=@public_key.pem"
curl -X POST 127.0.0.1:3002/upload/ec/private -H "Content-Type: multipart/form-data" -F "file=@private_key.pem"
```

## Zadanie 3.3 - Weryfikacja kluczy RSA
1) Wysłanie kluczy RSA do serwera:
```bash
curl -X POST 127.0.0.1:3003/checkkeys -H "Content-Type: multipart/form-data" -F "private_key_pem=@private_key.pem" -F "public_key_pem=@public_key.pem"
```

## Zadanie 3.4 - Generowanie kluczy RSA i publicznego z klucza prywatnego
1) Generowanie klucza publicznego z prywatnego:
```bash
openssl rsa -in private_key.pem -pubout -out public_key.pem
```
2) Wysłanie klucza publicznego:
```bash
curl -X POST 127.0.0.1:3004/submit -H "Content-Type: application/json" -d '{"session_id":"<session_id>", "public_key_pem":"@public_key.pem"}'
```
# Password cracking

## Zadanie 4.1 - Łamanie MD5
1) Wysłanie requestu do endpointa:
```bash
curl 127.0.0.1:4001/hash
```
2) Łamanie hash MD5:
```bash
hashcat -m 0 <hash_file> /path/to/rockyou.txt
```

## Zadanie 4.2 - Łamanie SHA-1
1) Wysłanie requestu do endpointa:
```bash
curl 127.0.0.1:4002/hash
```
2) Łamanie hash SHA-1:
```bash
hashcat -m 100 <hash_file> /path/to/rockyou.txt
```

## Zadanie 4.3 - Łamanie SHA-256
1) Wysłanie requestu do endpointa:
```bash
curl 127.0.0.1:4003/hash
```
2) Łamanie hash SHA-256:
```bash
hashcat -m 1400 <hash_file> /path/to/rockyou.txt
```

## Zadanie 4.4 - Łamanie SHA-512
1) Wysłanie requestu do endpointa:
```bash
curl 127.0.0.1:4004/hash
```
2) Łamanie hash SHA-512:
```bash
hashcat -m 1800 <hash_file> /path/to/rockyou.txt
```

## Zadanie 4.5 - Łamanie bcrypt
1) Wysłanie requestu do endpointa:
```bash
curl 127.0.0.1:4005/hash
```
2) Łamanie bcrypt:
```bash
hashcat -m 3200 <hash_file> /path/to/rockyou.txt
```

## Zadanie 4.6 - Łamanie SHA-1 z zastosowaniem słownika
1) Wysłanie requestu do endpointa:
```bash
curl 127.0.0.1:4006/hash
```
2) Łamanie SHA-1:
```bash
hashcat -m 100 <hash_file> /path/to/wordlist.txt
```

## Zadanie 4.7 - Łamanie MD5
1) Wysłanie requestu do endpointa:
```bash
curl 127.0.0.1:4007/hash
```
2) Łamanie MD5:
```bash
hashcat -m 0 <hash_file> /path/to/rockyou.txt
```

## Zadanie 4.8 - Łamanie SHA-1 z zastosowaniem słownika
1) Wysłanie requestu do endpointa:
```bash
curl 127.0.0.1:4008/hash
```
2) Łamanie SHA-1:
```bash
hashcat -m 100 <hash_file> /path/to/wordlist.txt
```

## Zadanie 4.9 - Łamanie SHA-256
1) Wysłanie requestu do endpointa:
```bash
curl 127.0.0.1:4009/hash
```
2) Łamanie SHA-256:
```bash
hashcat -m 1400 <hash_file> /path/to/rockyou.txt
```

## Zadanie 4.10 - Łamanie SHA-512
1) Wysłanie requestu do endpointa:
```bash
curl 127.0.0.1:4010/hash
```
2) Łamanie SHA-512:
```bash
hashcat -m 1800 <hash_file> /path/to/rockyou.txt
```
# GPG

## Zadanie 5.1 - RSA i eksport kluczy
1) Generowanie klucza RSA:
```bash
gpg --gen-key
```
2) Eksport kluczy:
```bash
gpg --armor --export -o pub.key
gpg --armor --export-secret-keys -o priv.key
```
3) Wysłanie kluczy na serwer:
```bash
curl -X POST 127.0.0.1:5001/submit/public -F "file=@pub.key"
curl -X POST 127.0.0.1:5001/submit/private -F "file=@priv.key"
```

## Zadanie 5.2 - DSA i eksport kluczy
1) Generowanie klucza DSA:
```bash
gpg --gen-key
```
2) Eksport kluczy:
```bash
gpg --armor --export -o pub.key
gpg --armor --export-secret-keys -o priv.key
```
3) Wysłanie kluczy na serwer:
```bash
curl -X POST 127.0.0.1:5002/submit/public -F "file=@pub.key"
curl -X POST 127.0.0.1:5002/submit/private -F "file=@priv.key"
```

## Zadanie 5.3 - RSA z określoną długością i datą ważności
1) Generowanie klucza RSA:
```bash
gpg --gen-key
```
2) Eksport kluczy:
```bash
gpg --armor --export -o pub.key
gpg --armor --export-secret-keys -o priv.key
```
3) Wysłanie kluczy na serwer:
```bash
curl -X POST 127.0.0.1:5003/submit/public -F "file=@pub.key"
curl -X POST 127.0.0.1:5003/submit/private -F "file=@priv.key"
```

## Zadanie 5.4 - Deszyfrowanie pliku (CAMELLIA128)
1) Pobranie pliku:
```bash
curl -X GET 127.0.0.1:5004/decrypt -o encrypted.zip
```
2) Rozpakowanie pliku:
```bash
unzip encrypted.zip
```
3) Deszyfrowanie:
```bash
gpg --batch --yes --cipher-algo CAMELLIA128 --passphrase $(cat passphrase.txt) --decrypt encrypted.txt
```

## Zadanie 5.5 - AES128 i szyfrowanie
1) Pobranie pliku:
```bash
curl -X GET 127.0.0.1:5005/encrypt -o encrypted_word.zip
```
2) Szyfrowanie:
```bash
gpg --batch --yes --armor --cipher-algo AES128 --passphrase $(cat passphrase) --encrypt encrypted_word
```

## Zadanie 5.6 - Importowanie klucza i szyfrowanie
1) Zaimportowanie klucza:
```bash
gpg --import public_key.asc
```
2) Szyfrowanie:
```bash
gpg --armor --encrypt --recipient <recipient_email> encrypted_word.txt
```

## Zadanie 5.7 - Deszyfrowanie i weryfikacja
1) Pobranie pliku:
```bash
curl -X GET 127.0.0.1:5007/decrypt -o encrypted.zip
```
2) Rozpakowanie:
```bash
unzip encrypted.zip
```
3) Deszyfrowanie:
```bash
gpg --decrypt encrypted_word.asc
```

## Zadanie 5.8 - Podpisywanie i weryfikacja podpisu
1) Pobranie pliku:
```bash
curl -X GET 127.0.0.1:5008/sign -o sign.zip
```
2) Podpisanie:
```bash
gpg --sign word.txt
```

## Zadanie 5.9 - Podpisanie i weryfikacja podpisu z oddzielnym podpisem
1) Pobranie pliku:
```bash
curl -X GET 127.0.0.1:5009/detach-sign -o sign.zip
```
2) Podpisanie:
```bash
gpg --detach-sign word.txt
```

## Zadanie 5.10 - Weryfikacja podpisu
1) Pobranie pliku:
```bash
curl -X GET 127.0.0.1:5010/clearsign -o clearsign.zip
```
2) Podpisanie:
```bash
gpg --clearsign word.txt
```

## Zadanie 5.11 - Weryfikacja podpisu
1) Pobranie pliku:
```bash
curl -X GET 127.0.0.1:5011/sign -o sign.zip
```
2) Zweryfikowanie podpisu:
```bash
gpg --verify word.txt.gpg
```
# Powtórzenie
# Kolokwium
