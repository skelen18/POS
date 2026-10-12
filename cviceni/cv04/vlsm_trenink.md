 # VLSM: interaktivní pracovní sešit

Pracovní list k procvičení VLSM a určování typu IPv4 adresy před zkouškou z předmětu Počítačové sítě na VŠB-FEI.

## 1. Rychlý tahák

### Pětivteřinový postup

Pro IP adresu ve tvaru `a.b.c.d/m`:

1. **Velikost bloku:** spočítej počet bitů pro hosty `h = 32 - m` a potom `2^h`.
2. **Síť:** najdi násobky velikosti bloku v oktetu, kde maska končí. IP spadá do intervalu od jeho dolní hranice.
3. **Broadcast:** `adresa sítě + velikost bloku - 1`.
4. **Hosté:** všechny adresy mezi sítí a broadcastem, tedy `síť + 1` až `broadcast - 1`.
5. **Verdikt:** IP se rovná adrese sítě -> **Síť**, IP se rovná broadcastu -> **Broadcast**, jinak -> **Host**.

U masek `/8`, `/16` a `/24` je velikost bloku 256 v prvním oktetu za prefixem. U `/25` až `/32` se obvykle pracuje s posledním oktetem.

### Vzorový příklad

Urči typ adresy `192.168.1.173/27`.

- Hostitelských bitů: `32 - 27 = 5`.
- Velikost bloku: `2^5 = 32`.
- Násobky 32 v posledním oktetu: `0, 32, 64, 96, 128, 160, 192, 224`.
- Číslo `173` leží v intervalu `160-191`.
- Síť: `192.168.1.160`.
- Broadcast: `192.168.1.191`.
- Použitelní hosté: `192.168.1.161-192.168.1.190`.
- **Verdikt:** `173` je mezi sítí a broadcastem, takže jde o **Host**.

Komentář: U každé úlohy nejdřív najdi hranice bloku. Teprve potom porovnávej zadanou IP s adresou sítě a broadcastem.

## 2. Úlohy k vyplnění

### Část A: Typ adresy

U každého zadání doplň velikost bloku, adresu sítě, broadcast a verdikt. Do polí piš i jednotky nebo prefix podle zadání.

#### A1

Zadání: `192.168.1.96/27`

| Velikost bloku | Síť | Broadcast | Verdikt (Síť / Host / Broadcast) |
| :---: | :--- | :--- | :---: |
| __________ | __________________ | __________________ | __________________ |

#### A2

Zadání: `10.0.0.127/25`

| Velikost bloku | Síť | Broadcast | Verdikt (Síť / Host / Broadcast) |
| :---: | :--- | :--- | :---: |
| __________ | __________________ | __________________ | __________________ |

#### A3

Zadání: `172.16.5.70/26`

| Velikost bloku | Síť | Broadcast | Verdikt (Síť / Host / Broadcast) |
| :---: | :--- | :--- | :---: |
| __________ | __________________ | __________________ | __________________ |

#### A4

Zadání: `192.168.10.32/28`

| Velikost bloku | Síť | Broadcast | Verdikt (Síť / Host / Broadcast) |
| :---: | :--- | :--- | :---: |
| __________ | __________________ | __________________ | __________________ |

#### A5

Zadání: `10.10.10.255/24`

| Velikost bloku | Síť | Broadcast | Verdikt (Síť / Host / Broadcast) |
| :---: | :--- | :--- | :---: |
| __________ | __________________ | __________________ | __________________ |

### Část B: Mini-VLSM rozpad

Zadaný rozsah: `192.168.50.0/24`

Požadavky:

- LAN 1: 60 PC + 1 rozhraní routeru
- LAN 2: 25 PC + 1 rozhraní routeru
- P2P spoj: 2 rozhraní routerů

Postupuj od největší podsítě po nejmenší. U každé podsítě započítej adresu sítě a broadcast. U P2P spoje počítej se 2 použitelnými adresami a blokem 4 adres (`/30`).

| Požadavek | Velikost bloku | Maska | Adresa sítě | Rozsah hostů | Broadcast |
| :--- | :---: | :---: | :--- | :--- | :--- |
| LAN 1: 60 PC + 1 router | __________ | __________ | __________________ | __________________ | __________________ |
| LAN 2: 25 PC + 1 router | __________ | __________ | __________________ | __________________ | __________________ |
| P2P: 2 rozhraní routerů | __________ | __________ | __________________ | __________________ | __________________ |

## 3. Kontrola před odevzdáním

- [ ] Podsítě jsou seřazené od největší po nejmenší.
- [ ] Žádné dvě podsítě se nepřekrývají.
- [ ] Každá adresa sítě je na začátku svého bloku.
- [ ] Broadcast je poslední adresa svého bloku.
- [ ] Hostitelský rozsah neobsahuje adresu sítě ani broadcast.

## Prompt pro kontrolu odpovědí

Až doplníš odpovědi, pošli mi tento prompt:

> Zkontroluj moje vyplněné odpovědi v souboru `cviceni/cv04/vlsm_trenink.md`. U každé chyby napiš správný výpočet krok za krokem a stručně vysvětli, kde jsem se spletl. Zkontroluj také, zda se VLSM podsítě nepřekrývají a zda jsou správně seřazené od největší po nejmenší.
