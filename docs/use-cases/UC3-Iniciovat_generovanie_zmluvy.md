---
title: "UC3 – Iniciovať generovanie zmluvy"
date: 2025-06-18
author: "Peter Vidlička"
---

# UC3: Iniciovať generovanie zmluvy

**Primárny aktér:** Onboarding Modul  
**Sekundárny aktér:** GeneratorSmluv  
**Cieľ:** Onboarding Modul požiada externú službu GeneratorSmluv o vygenerovanie PDF zmluvy s predvyplnenými údajmi.

**Predpoklady:**  
- Klient úspešne dokončil UC2 (Nahratie dokladu).  
- Onboarding Modul má uložené všetky potrebné osobné údaje klienta.

---

## Hlavný scenár

1. Onboarding Modul zloží REST požiadavku na GeneratorSmluv:
   - Endpoint: `POST /contracts/generate`
   - Body: `{ clientData: {...} }`  
2. GeneratorSmluv spracuje požiadavku, vytvorí PDF a vráti ho v odpovedi.  
3. Onboarding Modul uloží prijaté PDF do objektového úložiska a v databáze zapíše referenciu (cestu, čas vygenerovania, stav „generated“).  

---

## Alternatívne scenáre

- **2a. Chyba pri generovaní PDF**  
  1. GeneratorSmluv vráti HTTP 5xx alebo chybu validačných dát.  
  2. Onboarding Modul zobrazí chybovú hlášku „Generovanie zmluvy zlyhalo, skúste neskôr“ a ponúkne tlačidlo „Opakovať“.  

- **1a. Neúplné alebo nevalidné vstupné dáta**  
  1. GeneratorSmluv vráti HTTP 4xx s popisom chybných polí.  
  2. Onboarding Modul zvýrazní problematické údaje a umožní opravu.