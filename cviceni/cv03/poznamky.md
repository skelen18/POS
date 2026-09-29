# Cvičení 3: Virtuální LAN (VLAN) a L3 topologie

## Tohle cviko neproběhlo, kvůli státnímu svátku, takže tohle je výcuc z prezentace, mozna jsem cooked

## Teorie k VLAN
- Oddělují provoz na spojové vrstvě (L2) a softwarově rozdělují broadcastové domény.
- Rámce se mezi různými VLAN nepropouští, síť se chová jako několik fyzicky oddělených sítí.
- **Trunk linky:** Slouží k propojení více přepínačů a přenosu dat z více VLAN po jednom kabelu. Do hlavičky rámce se přidává tag (standard 802.1q) s informací, do které VLAN rámec patří.

## Ekvivalentní L3 topologie (příprava na projekt a test)
- Překreslení fyzické sítě tak, jak ji vidí 3. vrstva OSI modelu (směrovače a IP adresace).
- Každá VLAN uvnitř fyzického switche se kreslí jako samostatný virtuální switch (např. označení `SW 1/10` znamená Switch 1 a VLAN 10).
- Trunk linky se v L3 schématech kreslí čárkovaně a čísla portů se k nim mohou zapisovat vícekrát pro různé VLAN.

## Konfigurace Cisco IOS (Catalyst 2960)

### Vytvoření a pojmenování VLAN
```text
enable
conf t
vtp mode transparent
vlan 10
 name SUPPORT
 exit
```

### Nastavení Access portu (připojení koncového PC)
```text
interface FastEthernet 0/1
 switchport mode access
 switchport access vlan 10
 exit
```

### Nastavení Trunk portu (propojení mezi switchi)
```text
interface range GigabitEthernet 0/1-2
 switchport mode trunk
 ! Povolení konkrétních VLAN na trunku (pokud nezadáme, projdou všechny):
 switchport trunk allowed vlan add 10,20
 exit
```
*(Poznámka: U L3 switchů jako 3560 je někdy nutné před zapnutím trunku zadat `switchport trunk encapsulation dot1q`)*.

### Užitečné diagnostické příkazy (privilegovaný režim)
```text
! Výpis existujících VLAN a do nich zařazených access portů
show vlan

! Výpis aktivních trunk linek
show interfaces trunk

! Výpis konfigurace jednoho konkrétního portu
show running-config interface fastethernet 0/1
show interfaces fastethernet 0/1 switchport
```

### Mazání VLAN
```text
! Smazání jedné konkrétní VLAN v konfiguračním režimu
no vlan 10

! Kompletní smazání databáze VLAN (vyžaduje restart switche)
delete vlan.dat
```