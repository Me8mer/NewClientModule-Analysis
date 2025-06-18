---
title: "UC9 – Požiadať SMTPServer o odoslanie e-mailu"
date: 2025-06-18
author: "Peter Vidlička"
---

# UC9: Požiadať SMTPServer o odoslanie e-mailu

**Primárny aktér:** OnboardingModul
**Sekundárny aktér:** SMTPServer
**Cieľ:** OnboardingModul odošle požiadavku na doručenie e-mailu klientovi prostredníctvom SMTP servera.

**Predpoklady:**
- Klient má zadaný platný e-mail.
- Z predchádzajúcich UC sú k dispozícii potrebné informácie (napr. potvrdenie o vytvorení účtu).
- SMTP server je dostupný.

---

## Hlavný scenár

1. **OnboardingModul** pripraví obsah e-mailu (napr. potvrdenie o založení účtu).
2. Zostaví požiadavku:
   - Príjemca: e-mail klienta
   - Predmet, telo e-mailu
   - Voliteľne príloha (napr. zmluva)
3. Odošle požiadavku cez SMTP klienta na **SMTPServer**.
4. Očakáva potvrdenie o prijatí správy na odoslanie.
5. Ak je správa prijatá, nastaví stav „odoslané“ alebo „v procese“.

---

## Alternatívne scenáre

- **3a. SMTPServer odmietne požiadavku (napr. 550, 451)**
  - OnboardingModul zaznamená chybu
  - Stav e-mailu označí ako „neodoslané“ a zobrazí info na dashboarde

- **4a. Nedoručenie / technická chyba**
  - SMTP klient nedostane žiadnu odpoveď (timeout, chyba DNS...)
  - OnboardingModul prepne stav na „zlyhané“, možnosť opätovného odoslania

