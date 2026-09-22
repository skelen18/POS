# Cvičení 2: Kabelážní systémy a Spanning Tree Protocol (STP)

## Metalická kabeláž
- Koaxiální kabel: konektory BNC, T-člen, terminátor (10BASE5 do 500 m, 10BASE2 do 185 m).
- Kroucená dvoulinka (Twisted pair): UTP (nestíněná), FTP (stíněná fólií), STP (stíněná opletením), konektor RJ-45.
- Konstrukce vodiče: lanko (patch kabely, ohebné) vs. drát (rozvody ve zdech, patch panely, zařezávací zásuvky).

## Zapojení konektoru RJ-45
Barevné schéma zapojení pinů zleva doprava (pohled na kontakty shora, plastová západka směřuje dolů):

| Pin | Norma TIA/EIA 568A | Norma TIA/EIA 568B |
| :-: | :--- | :--- |
| 1 | Bílo-zelená | Bílo-oranžová |
| 2 | Zelená | Oranžová |
| 3 | Bílo-oranžová | Bílo-zelená |
| 4 | Modrá | Modrá |
| 5 | Bílo-modrá | Bílo-modrá |
| 6 | Oranžová | Zelená |
| 7 | Bílo-hnědá | Bílo-hnědá |
| 8 | Hnědá | Hnědá |

### Typy kabelů
- Přímý kabel (Straight-through): na obou stranách stejná norma (v Evropě T568B). Propojení odlišných prvků (PC – Switch, Router – Switch).
- Křížený kabel (Crossover): na jedné straně T568A, na druhé T568B (křížení vysílacích a přijímacích párů 1,2 a 3,6). Propojení stejných prvků (PC – PC, Switch – Switch, Router – Router, Router – PC).
- Konzolový kabel (Rollover / Console): jedna strana T568B, druhá strana otočená o 180 stupňů (pin 1 na pin 8, pin 2 na pin 7 atd.). Používá se pro sériovou správu aktivních prvků.

## Spanning Tree Protocol (STP - IEEE 802.1D)
Protokol druhé vrstvy (L2) zajišťující redundanci linek bez vzniku fyzických smyček a broadcastových bouří.

1. Volba kořenového přepínače (Root Bridge): přepínač s nejnižší hodnotou Bridge ID (Bridge ID = konfigurovatelná priorita + MAC adresa přepínače).
2. Výpočet nejkratší cesty: každý přepínač určí nejkratší cestu k Root Bridge podle kumulativní ceny linek (port cost)- .
3. Role portů v topologii:
   - Root Port (RP): port s nejnižší cenou cesty k Root Bridge (právě jeden na každém přepínači kromě kořene).
   - Designated Port (DP): port přeposílající provoz do daného segmentu sítě (všechny porty na Root Bridge jsou designated).
   - Alternate / Blocked Port (BLK / ALT): port vyřazený ze směrování datových rámců pro zamezení vzniku smyčky.

## Konfigurace Cisco IOS (Catalyst 2960)

### Základní inicializace
```text
enable
configure terminal
no ip domain-lookup