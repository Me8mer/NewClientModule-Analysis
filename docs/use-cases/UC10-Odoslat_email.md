---
title: "UC10 – Odoslať e-mail"
date: 2025-06-18
author: "Peter Vidlička"
---

# UC10: Odoslať e-mail

**Primárny aktér:** SMTPServer
**Cieľ:** Server odošle e-mail príjemcovi na základe požiadavky prijatej z OnboardingModulu.

**Predpoklady:**
- E-mailová požiadavka bola prijatá a je validná.
- SMTPServer má prístup k sieti a MX záznamom.

---

## Hlavný scenár

1. **SMTPServer** prijme požiadavku s kompletnými údajmi (To, Subject, Body, prípadne príloha).
2. Vytvorí e-mailovú správu podľa SMTP štandardu.
3. Nadviaže spojenie s cieľovým mail serverom (napr. mail.google.com).
4. Odošle správu cez SMTP protokol.
5. Zaznamená odpoveď a stav:
   - 250 OK → úspech
   - 4xx / 5xx → chyba
6. Vráti odpoveď OnboardingModulu (ak má spätný kanál).

---

## Alternatívne scenáre

- **3a. Chyba v spojení s cieľovým serverom**
  - Timeout, DNS chyba, odmietnutie spojenia
  - E-mail sa označí ako „neodoslaný“, môže sa pokúsiť o re-try

- **5a. Permanentná chyba (napr. mailbox neexistuje)**
  - Stav sa uloží ako „zlyhané doručenie“
  - Odporúča sa logovanie a notifikácia operátora

---

## Poznámky

- V prípade potreby môže SMTPServer implementovať re-try mechanizmus (napr. po 5, 15 a 60 minútach).
- Môže sa logovať aj Message-ID, DKIM výsledky, resp. spätné chyby cez bounce správy.