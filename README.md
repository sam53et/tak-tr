# TAK-TR

VPS üzerinde SSH kullanıcı yönetimi, bağlantı limiti, süre takibi ve çoklu protokol (Squid, Dropbear, Stunnel, Socks, SSLH, BadVPN) desteği.

## Kurulum

```bash
bash <(curl -Ls https://raw.githubusercontent.com/sam53et/tak-tr/main/install.sh)
```

Kurulum sonrası:

```bash
menu
# veya
h
```

## Paket Kurulumları

Script paketleri **üç farklı şekilde** yönetir.

---

### 1. Kurulum sırasında otomatik kurulan paketler

`install.sh` çalıştırıldığında eksik olanlar `apt-get` ile otomatik kurulur.

**Temel araçlar (DEPS_INSTALLER)**
- `curl`
- `wget`
- `ca-certificates`
- `tar`

**Çalışma zamanı bağımlılıkları (DEPS_RUNTIME)**
- `screen`
- `nano`
- `zip`
- `lsof`
- `net-tools`
- `nload`
- `jq`
- `python3`
- `iproute2`
- `cron`

**Sunucu ayarları için (isteğe bağlı – DEPS_SERVER_SETTINGS)**
- `systemd-sysv`
- `tzdata`

> Not: `list` dosyasında `jq` sadece `apt-get install -y jq` ile kurulur. Eski binary indirme + üzerine yazma yöntemi kaldırılmıştır.

---

### 2. Menüden / ihtiyaç halinde otomatik kurulan paketler

Kullanıcı ilgili özelliği **ilk kez açtığında** script kendisi kurar:

| Özellik              | Kurulan paket(ler)                                      | Ne zaman kurulur                          |
|----------------------|---------------------------------------------------------|-------------------------------------------|
| Dropbear             | `dropbear`                                              | Dropbear menüsünden açılınca              |
| Squid Proxy          | `squid`                                                 | Squid menüsünden açılınca                 |
| Stunnel              | `stunnel4`                                              | Stunnel menüsünden açılınca               |
| SSLH                 | `sslh`                                                  | SSLH menüsünden açılınca                  |
| Socks Proxy          | Ek paket yok (`python3` + `proxy.py` kullanılır)        | Socks menüsünden açılınca                 |
| BadVPN UDPGW         | `git`, `cmake`, `build-essential` + kaynak kod derleme  | UDPGW menüsünden açılınca                 |
| Multiport (eksikse)  | `openssh-server`, `iproute2`, `grep`, `sed`, `gawk`, `coreutils` | Multiport kullanılırken          |

> Multiport modülü **apt / yum / dnf / pacman** destekler.  
> Diğer protokoller şu an sadece **apt** (Debian/Ubuntu) varsayımıyla çalışır.

---

### 3. Manuel kurulum gereken / sadece uyarı verilen durumlar

Script bunları her zaman otomatik kurmaz; eksik kalırsa uyarı verir:

- **nload**  
  Normal kurulumda `DEPS_RUNTIME` içinde otomatik kurulur.  
  Ancak bir şekilde eksik kalırsa `vpstraffic` uyarı verir.  
  Manuel: `apt install nload`

- **jq**  
  Speedtest modülünde eksikse uyarı çıkar (normal kurulumda zaten otomatik kurulur).  
  Manuel: `apt install jq`

---

## Kaldırma

```bash
removescript
```

## Notlar

- Root yetkisi gereklidir.
- Debian / Ubuntu önerilir.
- Diğer dağıtımlarda (CentOS, Fedora, Arch) multiport hariç çoğu paket kurulumu sadece apt varsayımıyla çalışır.
- UFW desteği kaldırılmıştır. Portları güvenlik duvarında manuel açmanız gerekir.
