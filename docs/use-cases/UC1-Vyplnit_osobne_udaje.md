---
title: "UC1 - Vyplniť osobné údaje"
date: 2025-06-18
author: "Peter Vidlička"
---

# UC1: Vyplneniť osobné údaje

**Primárny aktér:** Klient  
**Cieľ:** Klient zadá všetky povinné osobné údaje potrebné pre založenie účtu online.  

**Predpoklady:**  
- Klient otvoril webovú stránku/modul na založenie nového účtu.  
- Klient má funkčné internetové pripojenie a prehliadač.  

---


## Hlavný scenár

1. Systém zobrazí formulár so vstupnými poliami:
   - Meno  
   - Priezvisko  
   - Rodné číslo  
   - E-mail  
   - Telefón  
   - Korespondenčná adresa  
   - Trvalé bydlisko  
2. Klient vyplní všetky povinné polia.  
3. Systém validuje formáty a obsah:
   - Overí, či je e-mail v platnom tvare.  
   - Skontroluje formát rodného čísla.  
   - Skontroluje, či nie sú prázdne povinné polia.  
4. Ak sú všetky údaje správne:
   1. Systém uloží údaje do PostgreSQL.  
   2. Systém zobrazí potvrdzovaciu správu „Údaje úspešne uložené“ a prepne užívateľa na ďalší krok (nahratie dokladu).  

---

## Alternatívne scenáre

- **3a. Nevalidný e-mail**  
  1. Systém zobrazí chybovú hlášku „Zadajte platný e-mail“.  
  2. Klient opraví e-mail a zopakuje odoslanie formulára.  

- **3b. Nesprávny formát rodného čísla**  
  1. Systém zvýrazní pole „Rodné číslo“ s oznamom „Neplatný formát rodného čísla“.  
  2. Klient opraví rodné číslo a odoslanie pokračuje.  

- **2a. Prázdne povinné pole**  
  1. Systém označí prázdne pole a zobrazí „Toto pole je povinné“.  
  2. Klient vyplní chýbajúce údaje a opätovne odošle formulár.  
