---
title: "UC2 – Nahrať doklad totožnosti"
date: 2025-06-18
author: "Peter Vidlička"
---

# UC2: Nahrať doklad totožnosti

**Primárny aktér:** Klient  
**Cieľ:** Klient nahraje sken dokladu totožnosti (občiansky preukaz, vodičský preukaz alebo cestovný pas).  

**Predpoklady:**  
- Klient úspešne dokončil UC1 (Vyplnenie osobných údajov) a je presmerovaný na obrazovku nahratia dokladu.  
- Klient má pripravený digitálny súbor dokladu v podporovanom formáte (JPEG, PNG, PDF).  

---

## Hlavný scenár

1. Systém zobrazí formulár s ovládacím prvkom na výber a nahratie súboru.  
2. Klient zvolí súbor so skenom dokladu a potvrdí odoslanie.  
3. Systém:
   1. Overí typ a veľkosť súboru (max. napr. 5 MB, formát JPEG/PNG/PDF).  
   2. Uloží súbor do objektového úložiska (napr. S3 alebo interné file store).  
   3. V databáze (PostgreSQL) uloží referenciu na uložený súbor (cestu, URI, metadáta).  
4. Systém zobrazí potvrdenie „Doklad bol úspešne nahraný“ a presmeruje klienta na UC3 (Generovanie a podpis zmluvy).  

---

## Alternatívne scenáre

- **3a. Nepodporovaný formát alebo prekročená veľkosť**  
  1. Systém odmietne súbor, zobrazí chybové hlásenie „Formát súboru nie je podporovaný“ alebo „Súbor musí byť menší ako 5 MB“.  
  2. Klient vyberie iný súbor a zopakuje upload.  

- **3b. Chyba pri uložení do úložiska**  
  1. Systém zachytí internú chybu (timeout, nedostupné úložisko), zobrazí „Chyba pri nahrávaní, skúste to prosím neskôr“.  
  2. Klient môže zopakovať akciu alebo pokračovať neskôr.  

