# Sunucu Proxy Kurulum : 


![resim](https://github.com/user-attachments/assets/26031c4a-46d9-4bbe-9814-ddce9baa480a)


##### Ücretsiz 10 Adet Günlük Proxy : https://www.webshare.io/?referral_code=xn5elzbhniup

```bash
nano ~/.bashrc
```

#### 2 Farklı Proxy Kullanabiliriz.

- HTTP
- HTTPS

#### Proxy Aldığınız şirket / sağlayıcıda özellikleri yazar. Bizim paylaştığımız WEBSHARE = HTTPS !

## HTTP İçin : 

```bash
export http_proxy="http://KULLANICI_ADI:SIFRE@PROXY_IP:PORT"
```

## HTTPS İçin : 

```bash
export https_proxy="https://KULLANICI_ADI:SIFRE@PROXY_IP:PORT"
```

#### Örneğin, proxy bilgileriniz şu şekilde olsun:

- Proxy IP: 192.168.1.1
- Proxy Portu: 8080
- Kullanıcı adı: user
- Şifre: password


```bash
export https_proxy="https://user:password@192.168.1.1:8080"
```

#### Bilgilendirme : Bu bilgilerinin tümü sizin panelinizde olacaktır. 

#### Düzenlenmiş halini nano ile açtığımız dosyanın en sonuna gelip sağ tık ile yapıştırın.

```bash
nano ~/.bashrc
``` 
![resim](https://github.com/user-attachments/assets/dabe4720-8ec7-42ae-85e0-4461c8d13a46)

- CTRL X CTRL Y Enter. Kayıt Edin.

#### Ayarlayı Uygulayalım / Sisteme Proxy'i Yedirelim : 

```bash
source ~/.bashrc
``` 

#### Nasıl Anlarız Sistem Proxy'i Kabul Etti ?

```bash
wget -qO- https://httpbin.org/ip
```

#### Sonucunda artık Sunucu İP'niz yerine Proxy İP'niz görükmelidir.

![resim](https://github.com/user-attachments/assets/c4b096e6-8f9f-4eef-bf95-99cd8a2777b8)


#### Docker'da Proxy Nasıl Yapılır ? 

Dockerinizin başlatmak seçeneceklerine yada docker-compose.yml dosyasında bulunan evoriment kısmına proxy'inizi ekleyebilirsiniz.

- environment bazen -e olarak geçer. Docker compose'da ise environment: kısmınan altında - çekilir boşluk bıralık proxy yapıştırılır.

#### Docker Run'lu versiyon - burada artık hangisini kullanacaksanız onu seçebilirsiniz. http kullanıyorsanız https'yi silin. https'yi kullanıyorsanız http'li kısmı silin.

```bash
docker run -d \
  -e http_proxy="http://user:password@PROXY_IP:PORT" \
  -e https_proxy="http://user:password@PROXY_IP:PORT" \
  --name chromium-container \
  chromium-image
```

#### HTTPS : 

```bash
docker run -d \
  -e https_proxy="http://user:password@PROXY_IP:PORT" \
  --name chromium-container \
  chromium-image
```

#### HTTP: 

```bash
docker run -d \
  -e http_proxy="http://user:password@PROXY_IP:PORT" \
  --name chromium-container \
  chromium-image
```


#### Docker Compose.yml'de Nasıl Yapılır ? 

- Örnek Chromium Docker Compose yml : https://github.com/FurkanL0/Extention

- Burada environment: kısmınan altında yerleştiriyoruz. 

#### Son hali böyle oluyor :

#### HTTPS : 

```bash
---
services:
  chromium:
    image: lscr.io/linuxserver/chromium:latest
    container_name: chromium
    security_opt:
      - seccomp:unconfined #optional
    environment:
      - CUSTOM_USER=username
      - PASSWORD=password
      - PUID=1000
      - PGID=1000
      - TZ=Europe/Berlin
      - CHROME_CLI=https://github.com/FurkanL0 #optional
      # Proxy ayarları
      - https_proxy=http://user:password@PROXY_IP:PORT
    volumes:
      - /root/chromium/config:/config
    ports:
      - 3010:3000   # Change 3010 to your favorite port if needed
      - 3011:3001   # Change 3011 to your favorite port if needed
    shm_size: "1gb"
    restart: unless-stopped
```

#### HTTP : 

```bash
---
services:
  chromium:
    image: lscr.io/linuxserver/chromium:latest
    container_name: chromium
    security_opt:
      - seccomp:unconfined #optional
    environment:
      - CUSTOM_USER=username
      - PASSWORD=password
      - PUID=1000
      - PGID=1000
      - TZ=Europe/Berlin
      - CHROME_CLI=https://github.com/FurkanL0 #optional
      # Proxy ayarları
      - http_proxy=http://user:password@PROXY_IP:PORT
    volumes:
      - /root/chromium/config:/config
    ports:
      - 3010:3000   # Change 3010 to your favorite port if needed
      - 3011:3001   # Change 3011 to your favorite port if needed
    shm_size: "1gb"
    restart: unless-stopped
```


