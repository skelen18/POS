# POS - Kompletní příprava na 1. test (L1 - L3, VLSM, Teorie)

---

## 1. Rozbor typického testu (10 bodů celkem)

Loňský test se skládal ze 4 úloh:
1. **VLSM rozpad a návrh podsítí** (4 body)
2. **Metody přístupu k médiu a kolize** (2 body)
3. **Kolizní a broadcastové domény (Hub vs Switch vs Router)** (2 body)
4. **Délky adres (IPv6, IPv4, MAC)** (2 body)

---

## 2. Úloha 1: VLSM (4 body) – Nauč se za 15 minut!

### Pravidlo pro výpočet potřebné velikosti podsítě
Vždy počítej: **počet PC + 1 IP adresa pro rozhraní routeru (výchozí bránu)**.
K tomu připočti **2 adresy (1 pro adresu sítě a 1 pro broadcast)**.
Hledáš **nejbližší vyšší mocninu 2**, která toto číslo pokryje:

| Počet stanic (+ router) | Celkem s rezervou (+2) | Velikost bloku ($2^h$) | Počet bitů $h$ | Maska | Prefix |
| :--- | :--- | :--- | :--- | :--- | :--- |
| P2P spoj (2 routery) | $2 + 2 = 4$ | **4** | 2 | 255.255.255.252 | **/30** |
| do 6 stanic | $6 + 2 = 8$ | **8** | 3 | 255.255.255.248 | **/29** |
| do 14 stanic | $14 + 2 = 16$ | **16** | 4 | 255.255.255.240 | **/28** |
| do 30 stanic | $30 + 2 = 32$ | **32** | 5 | 255.255.255.224 | **/27** |
| do 62 stanic | $62 + 2 = 64$ | **64** | 6 | 255.255.255.192 | **/26** |
| do 126 stanic | $126 + 2 = 128$ | **128** | 7 | 255.255.255.128 | **/25** |
| do 254 stanic | $254 + 2 = 256$ | **256** | 8 | 255.255.255.0 | **/24** |

### Postup řešení krok za krokem:
1. **Seřaď požadavky OD NEJVĚTŠÍHO PO NEJMENŠÍ!** (Kritické pravidlo VLSM – nikdy nezačínej malou podsítí).
2. Pro každou skupinu spočítej velikost bloku ($2^h$).
3. **Maska (prefix):** $32 - h$.
4. **Adresa sítě:** Začíná od zadané báze (nebo hned za předchozím broadcastem).
5. **První použitelná:** `Adresa sítě + 1`.
6. **Broadcast:** `Adresa sítě + velikost bloku - 1`.
7. **Poslední použitelná:** `Broadcast - 1`.
8. Další podsíť začíná na: `předchozí Broadcast + 1`.

### Vzor z loňského testu:
Zadáno: `71.212.88.0/23`
- Skupina 1: 84 stanic -> potřebuje $84 + 1 = 85$ adres -> blok **128** (`/25`)
- Skupina 3: 36 stanic -> potřebuje $36 + 1 = 37$ adres -> blok **64** (`/26`)
- Skupina 2: 7 stanic -> potřebuje $7 + 1 = 8$ adres -> blok **16** (`/28`) *(Pozor! Blok 8 má jen 6 použitelných adres, proto musíme vzít 16!)*
- P2P spoje: 4 spoje (D, E, F, G) -> každý 2 routery -> blok **4** (`/30`)

Tabulka:
- **A (Skupina 1):** Síť `71.212.88.0/25`, Rozsah: `71.212.88.1` - `71.212.88.126`, Broadcast: `71.212.88.127`
- **B (Skupina 3):** Síť `71.212.88.128/26`, Rozsah: `71.212.88.129` - `71.212.88.190`, Broadcast: `71.212.88.191`
- **C (Skupina 2):** Síť `71.212.88.192/28`, Rozsah: `71.212.88.193` - `71.212.88.206`, Broadcast: `71.212.88.207`
- **D (P2P 1):** Síť `71.212.88.208/30`, Rozsah: `71.212.88.209` - `71.212.88.210`, Broadcast: `71.212.88.211`
- **E (P2P 2):** Síť `71.212.88.212/30`, Rozsah: `71.212.88.213` - `71.212.88.214`, Broadcast: `71.212.88.215`
- **F (P2P 3):** Síť `71.212.88.216/30`, Rozsah: `71.212.88.217` - `71.212.88.218`, Broadcast: `71.212.88.219`
- **G (P2P Provider):** Síť `71.212.88.220/30`, Rozsah: `71.212.88.221` - `71.212.88.222`, Broadcast: `71.212.88.223`

---

## 3. Úloha 2 & 3: Síťové prvky, kolize a domény

### Zlatá pravidla síťových prvků:
| Zařízení | Vrstva OSI | Kolizní doména | Broadcastová doména |
| :--- | :--- | :--- | :--- |
| **HUB (Rozbočovač)** | L1 (Fyzická) | **NEODDĚLUJE** (vše na hubu je 1 velká kolizní doména, half-duplex) | **NEODDĚLUJE** |
| **SWITCH (Přepínač)** | L2 (Linková) | **ODDĚLUJE** (každý port switche je samostatná kolizní doména) | **NEODDĚLUJE** (celý switch je 1 broadcastová doména) |
| **ROUTER (Směrovač)**| L3 (Síťová) | **ODDĚLUJE** | **ODDĚLUJE** (každé rozhraní routeru je samostatná broadcastová doména) |

### Chytáky z testu:
- **Detekce kolizí v Ethernetu:** `CSMA/CD` (Collision Detection).
- **Předcházení kolizím ve Wi-Fi:** `CSMA/CA` (Collision Avoidance).
- **Lze ve switchovaném Ethernetu zcela zamezit kolizím?**
  - **ANO!**
  - **Důvod:** Při použití přepínačů a režimu **Full-Duplex** má každý port svou vlastní kolizní doménu a vysílání i příjem probíhají po oddělených vodičích/vláknech. Kolize fyzicky nemohou vzniknout (CSMA/CD je ve full-duplexu vypnuto).
- **S čím zkoliduje PC2 na HUBu?**
  - HUB funguje jako opakovač – signál pošle všem. Pokud vysílají dvě stanice na stejném hubu najednou, signály se smíchají a **zkolidují spolu** (v testu: PC2 zkoliduje s PC3).

---

## 4. Úloha 4: Délky adres

- **MAC adresa:** **48 bitů** = **6 bajtů** (zápis např. `00:1A:2B:3C:4D:5E`, hexadecimální)
- **IPv4 adresa:** **32 bitů** = **4 bajty** (zápis dekadický s tečkami, např. `192.168.1.1`)
- **IPv6 adresa:** **128 bitů** = **16 bajtů** (zápis hexadecimální s dvojtečkami, např. `2001:db8::1`)

---

## 5. Odpovědi z testovací databáze (110 otázek rozdělených do okruhů)

### 1. Fyzická vrstva & Kódování
- **10Base-T kódování:** Manchester
- **Rozdíl Manchester vs NRZ:** Manchester má změnu signálu v každém taktu (uprostřed bitu) -> snadná synchronizace hodin; NRZ úroveň drží.
- **1000Base-T (Gigabit Ethernet):** PAM-5 (5-úrovňová pulzní modulace).
- **100Base-TX (Fast Ethernet):** 4B/5B kódování následované MLT-3.
- **Optické sítě:** Často NRZ.
- **Proč Cat6 místo Cat5:** Vyšší frekvence (až 250 MHz), lepší stínění/odolnost proti přeslechům (crosstalk).

### 2. Modulace
- **Účel modulace:** Úprava nosného signálu pro přenos digitálních dat kanálem.
- **Základní typy:** ASK (amplituda), FSK (frekvence), PSK (fáze).
- **QAM (Kvadraturní amplitudová modulace):** Kombinace ASK + PSK (mění se amplituda i fáze). Používá se ve Wi-Fi a kabelových sítích/DSL pro vysokou přenosovou rychlost.

### 3. Metody přístupu k médiu (MAC)
- **CSMA/CD:** Ethernet na kabelech (detekce kolize -> jam signál -> náhodné čekání backoff).
- **CSMA/CA:** Bezdrátové sítě Wi-Fi (nelze detekovat kolizi při vlastním vysílání -> čekání + potvrzování ACK).
- **Token Passing:** Deterministická metoda (stanice vysílá jen tehdy, když drží speciální rámec – token, např. Token Ring).
- **TDMA:** Time Division Multiple Access (časové sloty, např. GSM, satelity).
- **Multiplexování:** Sdílení jednoho přenosového média více toky (TDM časové, FDM frekvenční, WDM vlnové v optice).

### 4. Síťové topologie
- **Typy:** Sběrnice (bus), Hvězdice (star), Kruh (ring), Strom (tree), Mříž (mesh).
- **Hvězdice:** Nejčastější (vše vede do centrálního switche). Výhoda: výpadek jednoho kabelu neovlivní ostatní.
- **Sběrnice:** Jeden kabel, na koncích terminátory (zabraňují odrazům signálu). Nevýhoda: přerušení kabelu vyřadí celou síť.
- **Mesh (plně propojená):** Každý s každým, vysoká redundance, drahé (páteřní sítě).

### 5. Linková vrstva & Přepínače (Switch)
- **Princip switche:** Přepíná rámce na základě **cílové MAC adresy**.
- **CAM tabulka (tabulka MAC adres):** Mapuje `MAC adresa <-> číslo portu`. Učí se ze **zdrojových MAC adres** příchozích rámců.
- **Neznámá cílová MAC (Unicast Flooding):** Pokud switch MAC nezná, pošle rámec na všechny porty kromě příchozího.
- **Auto MDI-X:** Automaticky detekuje přímý / křížený kabel a otočí páry.
- **Multilayer switch (L3 switch):** Umí přepínat (L2) i směrovat mezi sítěmi (L3) hardwarovou rychlostí.

### 6. MAC adresace a ARP
- **MAC adresa:** Fyzická/linková adresa, 48 bitů. První 3 bajty = OUI (identifikátor výrobce), zbylé 3 bajty = unikátní sériové číslo adaptéru.
- **Broadcastová MAC:** `FF:FF:FF:FF:FF:FF` (přijmou ji všichni v dané VLAN/síti).
- **ARP (Address Resolution Protocol):** Slouží k zjištění MAC adresy na základě známé IPv4 adresy (`Kdo má IP X.X.X.X, řekněte stanici Y.Y.Y.Y`).

### 7. Ethernetový rámec
- **Minimální délka:** 64 bajtů (bez preamble/SFD). Nutné kvůli CSMA/CD, aby stanice stihla detekovat kolizi, než dorazí konec rámce.
- **Maximální délka (MTU):** Standardní payload je 1500 bajtů (celý rámec 1518 bajtů).
- **Pole hlavičky:** Cílová MAC (6B), Zdrojová MAC (6B), EtherType/Délka (2B), Data (46-1500B), FCS/CRC (4B kontrolní součet pro ověření neporušenosti dat).
- **Rozdíl Ethernet II vs IEEE 802.3:** Ethernet II má pole `EtherType` (určuje protokol vyšší vrstvy, např. 0x0800 pro IPv4), 802.3 má místo toho pole `Length`.

### 8. Spanning Tree Protocol (STP - IEEE 802.1D / RSTP - 802.1w)
- **K čemu slouží:** Zabraňuje vzniku smyček (loopů) na L2 při redundantním propojení switchů (předchází broadcast bouřím - broadcast storm).
- **Jak funguje:** Vybere **Root Bridge** (switch s nejnižším Bridge ID = priorita + MAC). Ostatní switche najdou nejkratší cestu k Root Bridge a redundantní porty logicky zablokují (Blocking).
- **BPDU (Bridge Protocol Data Unit):** Řídicí zprávy, kterými si switche vyměňují informace o topologii.
- **RSTP:** Rychlejší konvergence (přepnutí při výpadku v řádu milisekund namísto 30–50 sekund u STP).

### 9. VLAN (Virtuální LAN - IEEE 802.1Q)
- **Co je VLAN:** Logické rozdělení jednoho fyzického switche na více nezávislých podsítí.
- **Hlavní výhody:** Zmenšení broadcastových domén, zvýšení bezpečnosti, lepší organizace sítě.
- **Access port:** Netagovaný port určený pro koncovou stanici (PC neví, ve které VLAN se nachází).
- **Trunk port:** Tagovaný spoj (obvykle mezi switchi nebo switch-router), který přenáší více VLAN najednou. Každý rámec dostane **802.1Q tag** (4 bajty), kde je uloženo **VLAN ID** (12 bitů, rozsah 1-4094).
- **Inter-VLAN routing:** Komunikace mezi různými VLAN musí jít VŽDY přes prvek L3 (Router nebo L3 switch).

### 10. IPv4, IPv6 a servisní protokoly
- **IPv4:** 32 bitů, formát dekadický (4 oktety).
- **IPv6:** 128 bitů, hexadecimální formát, nepoužívá broadcast (pouze unicast, multicast, anycast).
- **NDP (Neighbor Discovery Protocol):** V IPv6 nahrazuje ARP (hledání MAC adres pomocí ICMPv6 Neighbor Solicitation).
- **ICMP:** Slouží pro diagnostiku a hlášení chyb (např. nástroje `ping`, `traceroute`, zprávy Destination Unreachable, TTL expired).
- **DHCP:** Automatická konfigurace síťových parametrů (IP adresa, maska, výchozí brána, DNS). Běží přes DORA proces (Discover, Offer, Request, Acknowledge).
- **NAT (Network Address Translation):** Překládá privátní IPv4 adresy (RFC 1918) na veřejnou IP adresu pro přístup do internetu.

### 11. OSI Referenční model – Enkapsulace a PDU
| Vrstva | Název | PDU (Jednotka dat) | Hlavní adresace / Protokoly |
| :---: | :--- | :--- | :--- |
| **L7** | Aplikační | **Data** | HTTP, DNS, DHCP, SSH, FTP |
| **L6** | Prezentační | **Data** | Šifrování, komprese (SSL/TLS, JPEG) |
| **L5** | Relační | **Data** | Řízení relace (sessions) |
| **L4** | Transportní | **Segment** (TCP) / **Datagram** (UDP) | Porty (TCP, UDP) |
| **L3** | Síťová | **Paket** (Packet) | IP adresy (IPv4, IPv6, ICMP, směrování) |
| **L2** | Linková | **Rámec** (Frame) | MAC adresy (Ethernet, 802.11 Wi-Fi, VLAN) |
| **L1** | Fyzická | **Bity / Signály** | Napětí, světelné pulzy, kabely, konektory |

- **Enkapsulace:** Proces balení dat – každá vrstva přidá svou hlavičku (header) směrem dolů (L7 -> L1).
- **Dekapsulace:** Odstraňování hlaviček při příjmu dat na cílové stanici směrem nahoru (L1 -> L7).
