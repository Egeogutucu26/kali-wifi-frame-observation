# Kali Linux'ta Wi-Fi Çerçevelerini Pasif Gözlemleme

Bu notlar, Kali Linux'ta kablosuz ağ arayüzünü kontrol etme ve **yalnızca sahibi olduğunuz ya da açık izniniz olan bir ağın** yayın trafiğini pasif olarak gözlemleme içindir. Bağlantı kesme, paket enjeksiyonu veya başka bir cihaza müdahale adımları içermez.

## Gereksinimler

```bash
sudo apt update
sudo apt install -y aircrack-ng wireless-tools
```

Kablosuz adaptörün monitor mode'u desteklemesi gerekir.

## Arayüzü kontrol et

```bash
iw dev
iwconfig
```

Örneklerde kablosuz arayüz adı `wlan0` kabul edilmiştir. Sisteminizde farklıysa kendi arayüz adınızı kullanın.

## Monitor mode'u başlat

```bash
sudo airmon-ng start wlan0
```

Komut çıktısında monitor mode arayüzünün adını kontrol edin. Bu genellikle `wlan0mon` olur. Sonraki örneklerde `IFACE` değerini çıktıda görünen gerçek arayüz adıyla eşleştirin.

```bash
IFACE="wlan0mon"
```

## Yakındaki Wi-Fi ağlarını pasif gözlemle

```bash
sudo airodump-ng "$IFACE"
```

## Yalnızca izinli olduğunuz erişim noktasını gözlemle

Aşağıdaki örnek değerleri kendi test ağınızın bilgileriyle değiştirin. BSSID ve kanal, airodump-ng çıktısında görülebilir.

```bash
BSSID="AA:BB:CC:DD:EE:FF"
KANAL="6"
sudo airodump-ng --bssid "$BSSID" --channel "$KANAL" "$IFACE"
```

İsterseniz gözlemi yerel bir dosyaya kaydedebilirsiniz:

```bash
sudo airodump-ng --bssid "$BSSID" --channel "$KANAL" --write wifi-gozlem "$IFACE"
```

## Monitor mode'u kapat

```bash
sudo airmon-ng stop "$IFACE"
sudo systemctl restart NetworkManager
```

Arayüz isimleri ve bazı seçenekler adaptör/sürücüye göre değişebilir. Gerçek BSSID'leri, istemci MAC adreslerini veya yakalama dosyalarını herkese açık depolara yüklemeyin; bunlar konum ve cihaz bilgisi açığa çıkarabilir.
