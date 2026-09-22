# Cvičení 1: Seznámení s laboratoří a základy Cisco IOS CLI

## Laboratorní prostředí
- OS Ubuntu: uživatel `cnap`, heslo `cisco`
- Root práva: `su`, heslo `cisco`
- Sériová konzole: `minicom -s` (přenosová rychlost 115200 8N1, vypnout Hardware Flow Control)
- Odchod z minicomu: `Ctrl+A`, pak `X`

## Linux - síťové nástroje
- Zjištění IP adres: `ip a` (legacy: `ifconfig`)
- Zjištění směrování: `ip route` (legacy: `route -n`)
- Test spojení: `ping -c 4 <ip>`
- Sledování cesty: `traceroute <ip>`
- Odchytávání provozu: `tcpdump -i eth0 -n` nebo GUI `wireshark`

## Teorie: Hub vs. Switch
- **Hub (L1):** Všesměrový opakovač. Rámec přijatý na jednom portu pošle na všechny ostatní. Sdílená kolizní doména.
- **Switch (L2):** Přepínač. Čte cílové MAC adresy a podle CAM tabulky přeposílá rámce pouze na cílový port. Odděluje kolizní domény.
- **Kabeláž:**
  - Křížený kabel: zařízení stejného typu (PC-PC, Switch-Switch, Router-Router, Router-PC).
  - Přímý kabel: zařízení různého typu (PC-Switch, Switch-Router).

## Cisco IOS - Režimy a přechody
- `Switch>` ... Neprivilegovaný uživatelský režim (jen základní diagnostika)
- `Switch#` ... Privilegovaný režim (zadáním příkazu `enable`)
- `Switch(config)#` ... Globální konfigurační režim (zadáním `configure terminal` / `conf t`)
- `Switch(config-if)#` ... Konfigurace rozhraní (např. `interface FastEthernet 0/1`)
- Opuštění režimu o úroveň výš: `exit`
- Okamžitý návrat do `#`: `end` nebo zkratka `Ctrl+Z`

## Základní příkazy z 1. cvičení
```text
enable
conf t
hostname SW-CISLO_SWITCHE-SW-CISLO_VE_SKUPINE
no ip domain-lookup

interface FastEthernet 0/1
 description Linka k PC
 duplex auto
 speed auto
 no shutdown
 exit

show interfaces status
show mac-address-table dynamic
clear mac-address-table dynamic
show cdp neighbors
show running-config