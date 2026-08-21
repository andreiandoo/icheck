# Product Overview

## Misiune

EU-VERIFIC transformă surse oficiale și proceduri administrative în verificări, rapoarte, monitorizare și documente ușor de folosit înaintea unei decizii comerciale sau patrimoniale.

## Promisiune

**Verifică. Apoi decide.**

## Poziționare

EU-VERIFIC nu este un simplu agregator de date publice. Produsul urmărește să traducă surse oficiale dispersate într-un răspuns orientat către decizie.

Exemple de întrebări pe care produsul trebuie să le rezolve:

- Pot să lucrez cu firma aceasta?
- Pot să îi ofer termen de plată?
- Pot să plătesc avansul?
- Este transportatorul autorizat?
- Este contractorul autorizat?
- Ce documente există pentru imobil?
- Cum obțin documentul oficial fără să navighez portalurile instituțiilor?

## Unități principale de produs

- Company
- Property
- Licensed Operator
- Official Document
- Monitoring Subscription

## Categorii de produse

- `INSTANT_CHECK`
- `AGGREGATED_REPORT`
- `OFFICIAL_DOCUMENT`
- `PROCEDURE`
- `MONITORING`

## Principii

### Source-first

Fiecare rezultat trebuie să indice sursa, momentul interogării și evidența brută din care a fost derivat.

### Explainable

Findings și scorurile viitoare trebuie să fie explicabile și reproducibile.

### Official-data semantics

Nu formulăm concluzii mai puternice decât permit datele oficiale.

### Async-first

Providerii externi sunt considerați potențial indisponibili și sunt integrați prin jobs, retries și reconciliation.

### No scraping as critical foundation

Un scraper neautorizat nu poate constitui infrastructura critică a unui produs comercial.

## Verticale

1. Companii
2. Imobile
3. Transportatori
4. Contractori / licențe
5. Documente oficiale
6. Finanțări europene

## Non-goals pentru MVP

- motor general de căutare a persoanelor fizice;
- scoring AI black-box;
- agregare SEAP;
- scraping ca fundație critică;
- microservicii;
- API public B2B înainte de stabilizarea API-ului intern.

## Metrici inițiale de produs

- conversion rate lookup → purchase;
- time-to-first-result;
- report completion rate;
- provider success rate;
- document-order completion rate;
- gross margin per SKU;
- repeat purchase rate;
- attach rate pentru monitoring;
- customer support cases per 100 orders.

## Principiu comercial

Nu vindem „date ale statului”. Vindem:

- acces simplificat;
- verificare;
- structurare;
- trasabilitate;
- interpretare deterministă;
- orchestrarea procedurii;
- monitorizare.
