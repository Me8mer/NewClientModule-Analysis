# Analýza modulu: Online založenie účtu

## 1. Kontext a účel
- **Cieľ:** Umožniť klientovi založiť účet plne online.
- **Hlavní aktéri:** Klient, Onboarding modul, GeneratorSmluv, BankCore, SMTP Server.

## 2. Tabulka aktérov

| Aktér                | Popis                                                 |
| -------------------- | ----------------------------------------------------- |
| **Klient**           | Nový zákazník pristupujúci k on­line formuláru.       |
| **Onboarding Modul** | Aplikačná logika, ktorá spracúva všetky kroky. Cieľový systém |
| **GeneratorSmluv**   | Externé REST API na generovanie + podpis zmlúv.       |
| **BankCore**         | Externé SOAP API na vytvorenie čísla účtu.            |
| **SMTP Server**      | Interný mail-server na odoslanie potvrdenia a zmluvy. |


## 3. Funkčné požiadavky
1. Vyplnenie osobných údajov
2. Nahratie dokladu totožnosti
3. Generovanie a podpis zmluvy
4. Vytvorenie účtu a zaslanie e-mailu

## 4. Use-Case diagram
```mermaid
usecaseDiagram
    actor Klient
    actor OnboardingModul
    actor GeneratorSmluv
    actor BankCore
    actor SMTPServer

    (UC1)  Klient --> Vyplniť osobné údaje
    (UC2)  Klient --> Nahrať doklad totožnosti
    (UC3)  OnboardingModul --> Iniciovať generovanie zmluvy
    (UC4)  GeneratorSmluv --> Generovať zmluvu
    (UC5)  OnboardingModul --> Iniciovať podpis zmluvy
    (UC6)  Klient --> Podpísať zmluvu
    (UC7)  OnboardingModul --> Požiadať BankCore o vytvorenie účtu
    (UC8)  BankCore --> Vytvoriť účet
    (UC9)  OnboardingModul --> Požiadať SMTPServer o odoslanie e-mailu
    (UC10) SMTPServer --> Odoslať e-mail
```
