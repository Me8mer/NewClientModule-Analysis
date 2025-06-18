---
title: "Textový návrh dátového modelu"
date: 2025-06-18
author: "Peter Vidlička"
---

# Textový návrh dátového modelu

Tento dokument obsahuje textový návrh entít, atribútov a ich vzťahov pre modul online založenia bankového účtu.

---

## Entita: Klient

**Popis:** Uchováva základné osobné údaje klienta.

| Atribút         | Typ           | Popis                        |
|-----------------|---------------|------------------------------|
| client_id       | UUID / INT    | Primárny kľúč                |
| meno            | VARCHAR       | Krstné meno                  |
| priezvisko      | VARCHAR       | Priezvisko                   |
| rodne_cislo     | VARCHAR       | Rodné číslo                  |
| email           | VARCHAR       | E-mailová adresa             |
| telefon         | VARCHAR       | Telefónne číslo              |
| adresa_trvala   | VARCHAR       | Trvalé bydlisko              |
| adresa_kor      | VARCHAR       | Korespondenčná adresa        |
| created_at      | TIMESTAMP     | Dátum registrácie            |

---

## Entita: Doklad totožnosti

**Popis:** Informácie o nahratom doklade klienta.

| Atribút         | Typ           | Popis                        |
|-----------------|---------------|------------------------------|
| doklad_id       | UUID / INT    | Primárny kľúč                |
| client_id       | UUID / INT    | Cudzí kľúč na Klient         |
| typ_dokladu     | VARCHAR       | Typ dokladu (OP, pas, VP)    |
| cislo_dokladu   | VARCHAR       | Číslo dokladu                |
| subor_uri       | TEXT          | Cesta k súboru               |
| uploaded_at     | TIMESTAMP     | Dátum nahrania               |

---

## Entita: Zmluva

**Popis:** Informácie o zmluve generovanej pre klienta.

| Atribút         | Typ           | Popis                        |
|-----------------|---------------|------------------------------|
| zmluva_id       | UUID / INT    | Primárny kľúč                |
| client_id       | UUID / INT    | Cudzí kľúč na Klient         |
| stav            | VARCHAR       | Stav zmluvy (vygenerovaná/podpísaná) |
| pdf_uri         | TEXT          | Cesta k PDF súboru           |
| podpisana_at    | TIMESTAMP     | Dátum podpisu (ak existuje)  |

---

## Entita: Účet

**Popis:** Vytvorený účet pre klienta.

| Atribút         | Typ           | Popis                        |
|-----------------|---------------|------------------------------|
| ucet_id         | UUID / INT    | Primárny kľúč                |
| client_id       | UUID / INT    | Cudzí kľúč na Klient         |
| iban            | VARCHAR       | Číslo účtu (IBAN)            |
| created_at      | TIMESTAMP     | Dátum vytvorenia účtu        |

---

## Entita: EmailLog

**Popis:** Log odoslaných e-mailov cez SMTP.

| Atribút         | Typ           | Popis                        |
|-----------------|---------------|------------------------------|
| email_id        | UUID / INT    | Primárny kľúč                |
| client_id       | UUID / INT    | Cudzí kľúč na Klient         |
| predmet         | VARCHAR       | Predmet e-mailu              |
| stav            | VARCHAR       | Stav (odoslané, chyba...)    |
| sent_at         | TIMESTAMP     | Čas odoslania                |
