---
title: "UC4 – Generovať Zmluvu"
date: 2025-06-18
author: "Peter Vidlička"
---

# UC4: Generovať Zmluvu

**Primárny aktér:** GeneratorSmluv  
**Cieľ:** Externá služba vygeneruje PDF zmluvu na základe prijatých údajov a vráti ho volajúcemu modulu.

**Predpoklady:**  
- GeneratorSmluv obdržal platnú požiadavku z UC3.  

---

## Hlavný scenár

1. GeneratorSmluv overí integritu a úplnosť dát z volajúcej aplikácie.  
2. Na základe údajov vygeneruje PDF zmluvu podľa šablóny.  
3. Uloží dočasnú verziu PDF do svojho úložiska.  
4. Vráti PDF ako binárny obsah v tele odpovede s HTTP statusom 200.  

---

## Alternatívne scenáre

- **2a. Chyba pri generovaní šablóny**  
  1. Ak šablóna neexistuje alebo je nekompatibilná, vráti HTTP 500.  
  2. Volajúci modul spracuje chybu podľa scenára 2a v UC3.  

- **3a. Interná chyba úložiska**  
  1. Ak sa nepodarí uložiť dočasný súbor, vráti HTTP 503.  
  2. Volajúci modul ponúkne opätovné volanie UC3.  