# Warmups Write-up: Nahamsec CTF - Naham Commencement 2025

##  Challenge Adı
**Naham Commencement 2025**

## Soru:

![alt text](simage-5.png)

### Aşamalar

Challenge da verilen URL:
```
http://challenge.nahamcon.com:30113
```

Challenge verilen bir web sitesinde kullanıcı bilgilerini bulup login olduktan sonra flagi okuyabilmek

## Kaynak Kodu İncelemesi

![alt text](simage-1.png)


![alt text](simage-2.png)

a fonksiyonu 16 harflik Caesar Cipher yapıyor

b fonksiyonu bir Vigenère şifrelemesi uygular value ve key değeri alır her iki harfin alfabedeki sırasını bulur toplar sonucu mod 26 ile sınırlar ve çıkan değerin karşılık geldiği harfi alarak şifreleme yapar Harf olmayan karakterler aynen korunur

buna göre tersine çevirip bir kod yazdım ve şifreleri çözdüm 


```
username:nahamsec
password:LetTheGamesBegin2025

```


# FLAG

![alt text](simage-10.png)