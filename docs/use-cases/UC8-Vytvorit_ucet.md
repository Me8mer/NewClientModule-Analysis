---
title: "UC8 – Vytvoriť účet"
date: 2025-06-18
author: "Peter Vidlička"
---

# UC8: Vytvoriť účet

**Primárny aktér:** BankCore  
**Cieľ:** BankCore vytvorí nový klientsky účet na základe údajov a zmluvy prijatej z OnboardingModulu.

**Predpoklady:**  
- Bola prijatá platná požiadavka z UC7.  
- Overenie údajov prebehlo úspešne.  

---

## Hlavný scenár

1. **BankCore** overí vstupné dáta (klient, typ účtu, podpis).  
2. Systém vygeneruje:
   - Interné ID účtu  
   - Číslo účtu (IBAN)  
   - Založí entitu účtu v databáze
3. BankCore vytvorí interný log operácie a označí stav ako „úspešné vytvorenie“.
4. Odošle späť do OnboardingModulu odpoveď:
   - `{ accountNumber, status: "created" }`
5. OnboardingModul uloží číslo účtu a označí stav ako „Účet vytvorený“.

---

## Alternatívne scenáre

- **1a. Validácia vstupov zlyhá**  
  - BankCore vráti „neplatné údaje“, operácia sa nezrealizuje  
  - Stav sa nastaví ako „zamietnuté“, OnboardingModul je o tom informovaný

- **2a. Interná chyba pri zakladaní účtu**  
  - Napr. výpadok databázy, chyba generovania IBAN  
  - Stav sa označí ako „zlyhané“, OnboardingModul zobrazí chybu

---

## Poznámky

- Tento krok môže byť implementovaný synchrónne (v rámci UC7) alebo asynchrónne cez správu/event.
- Ak je proces asynchrónny, odpoveď o úspechu prichádza oneskorene cez samostatné API alebo event bus.