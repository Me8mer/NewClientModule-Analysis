---
title: "UC5 – Iniciovať podpis zmluvy"
date: 2025-06-18
author: "Peter Vidlička"
---

# UC5: Iniciovať podpis zmluvy

**Primárny aktér:** OnboardingModul  
**Sekundárny aktér:** GeneratorSmluv  
**Cieľ:** OnboardingModul zabezpečí inicializáciu procesu podpisu vygenerovanej zmluvy cez externý systém GeneratorSmluv.

**Predpoklady:**  
- UC3 (Iniciovať generovanie zmluvy) a UC4 (Generovať zmluvu) boli úspešne dokončené.  
- Vygenerovaný dokument je uložený a dostupný.  
- Klient má dostupné kontaktné údaje (napr. telefónne číslo na doručenie podpisového kódu).

---

## Hlavný scenár

1. **OnboardingModul** pripraví požiadavku na **GeneratorSmluv**, ktorá obsahuje:
   - Identifikátor zmluvy (napr. UUID)
   - Identifikátor klienta
   - Telefónne číslo klienta na doručenie SMS
2. **OnboardingModul** vyšle REST požiadavku na endpoint GeneratorSmluv:
   - `POST /contracts/initiate-signing`
   - Body: `{ documentId, clientId, phoneNumber }`
3. **GeneratorSmluv**:
   - Vytvorí podpisový token
   - Zašle SMS s podpisovým kódom klientovi
   - Uloží stav dokumentu ako „awaiting_signature“
4. **GeneratorSmluv** vráti do OnboardingModulu potvrdenie o úspešnej inicializácii.
5. **OnboardingModul** uloží stav dokumentu (napr. „čaká na podpis“) a pripraví klientovi rozhranie na zadanie SMS kódu (UC6).

---

## Alternatívne scenáre

- **3a. Chyba pri doručení SMS**  
  1. GeneratorSmluv zistí chybu pri doručení (napr. nesprávne číslo, SMS brána neodpovedá).  
  2. GeneratorSmluv vráti chybu.  
  3. OnboardingModul zobrazí klientovi chybovú správu a ponúkne možnosť zadanie iného čísla alebo opakovanie pokusu.  

- **2a. Nevalidné vstupy (napr. neexistujúci documentId)**  
  1. GeneratorSmluv vráti chybu HTTP 400 s detailom problému.  
  2. OnboardingModul zobrazí technickú chybu operátorovi alebo klientovi (podľa kritickosti).  

- **4a. GeneratorSmluv nedostupný**  
  1. Timeout alebo HTTP 503.  
  2. OnboardingModul opakuje požiadavku alebo označí stav dokumentu ako „chybný podpis“ a zobrazí informáciu o technickej chybe.

---

## Poznámky

- Tento UC končí pripravením klienta na zadanie podpisového kódu (rieši UC6).
- Podpis samotný nevykonáva tento UC, ale len inicializuje podmienky na jeho vykonanie.
