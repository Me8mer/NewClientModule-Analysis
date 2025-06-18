---
title: "Nefunkčné požiadavky"
date: 2025-06-18
author: "Peter Vidlička"
---

# Nefunkčné požiadavky

Tento dokument stručne popisuje nefunkčné požiadavky kladené na modul pre online založenie účtu.

---

## 1. Perzistencia dát

- **Štruktúrované údaje** (napr. osobné údaje, doklady, stav zmluvy, číslo účtu) sú ukladané v **relačnej databáze PostgreSQL**, v súlade s požiadavkami zadania.
- **Neštruktúrované dáta** (napr. naskenované doklady, PDF zmluvy) sú uložené mimo databázy, napr.:
  - na súborový systém servera,
  - v objektovom úložisku (napr. S3 kompatibilnom),
  - URI odkazy sú evidované v databáze.

---

## 2. Zabezpečenie integrácií

- **Všetka komunikácia s externými systémami (GeneratorSmluv, BankCore, SMTP)** prebieha cez zabezpečené kanály (TLS / HTTPS / WSS).
- **REST API** (napr. GeneratorSmluv) odporúča použiť autorizáciu pomocou **OAuth2**, prípadne **API kľúče** s obmedzenou platnosťou.
- **SOAP API** (napr. BankCore) by malo využívať **WS-Security** štandardy (napr. podpisovanie hlavičiek, timestampy).
- Interné volania (napr. SMTP) môžu byť zabezpečené na sieťovej vrstve (VPN, TLS), s logovaním doručenia a retry logikou.

---

## 3. Spoľahlivosť a dostupnosť

- **Mechanizmy opakovania (retry)** sú implementované pri komunikácii so systémami BankCore a SMTP:
  - v prípade chyby sa požiadavka môže automaticky zopakovať s exponenciálnym oneskorením,
  - maximálny počet pokusov je definovaný (napr. 3x).
- **Logovanie a monitoring**:
  - Zaznamenáva sa výsledok každej akcie (napr. podpis, odoslanie e-mailu, vytvorenie účtu),
  - v prípade chyby sa stav označí a príslušná entita je evidovaná ako "čakajúca na spracovanie" alebo "neúspešná".

---

## 4. Škálovateľnosť a budúci rozvoj

- Modul je navrhnutý tak, aby bolo možné:
  - rozšíriť počet typov zmlúv (viac ako jeden typ / iterácia),
  - pridávať ďalšie kanály komunikácie (napr. notifikácie cez SMS gateway),
  - rozšíriť dátový model bez zásahu do existujúcich záznamov.

