# Open Questions

Acest document este registrul întrebărilor care pot schimba produsul, arhitectura sau legalitatea unei integrări. O întrebare rezolvată trebuie mutată într-un ADR, provider spec sau policy relevantă.

## ANCPI

1. Poate o societate comercială neautorizată cadastral să contracteze direct pachetele API relevante pentru property resolution?
2. Care sunt condițiile concrete de aprobare pentru P3/P4?
3. Ce documentație tehnică se oferă după aprobare?
4. Există sandbox/test environment?
5. Care este schema de autentificare?
6. Care sunt rate limits și SLA-urile?
7. Care este diferența semantică dintre P3 și API-urile dedicate Address/Parcel/Building?
8. Există canal M2M pentru comanda Extras CF/Copie CF sau doar UI/abonament?
9. Ce servicii pot fi depuse de un SRL ca deponent și care cer obligatoriu autorizat ANCPI?
10. Ce condiții se aplică identificării după proprietar și registrului proprietarilor?

## ONRC

1. Există M2M comercial pentru InfoCert?
2. Poate EU-VERIFIC comanda documente în numele clienților?
3. Care este contractul/tariful pentru acces recurent?
4. Există sandbox?
5. Există webhook/status API pentru documente?
6. Cum se gestionează interesul legitim pentru beneficiari reali într-un flux intermediat?
7. Ce informații pot fi reutilizate în rapoarte vs livrate doar ca document oficial?

## ARR

1. Există API/web service/feed pentru verificarea operatorilor?
2. Este permisă reutilizarea comercială a rezultatelor publice?
3. Există identificator stabil după CUI?
4. Sunt disponibile licențele, copiile conforme și numărul de vehicule machine-readable?
5. Care este update frequency?
6. Putem contracta monitoring/bulk access?

## ANRE

1. Care este licența/condiția de reutilizare a registrelor/exporturilor?
2. Există feed/API stabil în afara exportului UI?
3. Ce identificator unic și stabil există pentru firme/licențe?
4. Update frequency?
5. Historical data availability?
6. Există notificări/suspensions/revocations machine-readable?

## ANAF

1. Confirmarea endpointurilor și contractelor pe care le folosim în MVP.
2. Care capabilities cer OAuth/certificat și care sunt publice?
3. Rate limit per service, nu doar limita generală.
4. Politică oficială privind cache/reuse pentru fiecare registry.
5. Availability/versioning policy pentru serviciile web.
6. Semantica exactă a financial statements dataset la ani lipsă/rectificări.

## Justice

1. Există limitări/rate limits documentate pentru portalquery?
2. Cum gestionăm numele de firmă ambigue și schimbările de denumire?
3. Este disponibil CUI în vreun context de matching?
4. Care este politica recomandată de cache?
5. Cum distingem reliable party role/status în obiectele returnate?

## data.gov.ro / MIPE

1. Ce dataseturi au licență compatibilă și actualizare suficient de frecventă?
2. Există CUI stabil/normalizat pentru beneficiari?
3. Cum detectăm versiuni noi de resource fără full re-import inutil?
4. Care este freshness minim acceptabil pentru Funding Passport?

## Product

1. Payment processor pentru MVP?
2. e-Invoicing/invoicing strategy?
3. Guest checkout vs mandatory account per product?
4. Care checks sunt gratuite/freemium?
5. Primele pricing experiments?
6. PDF report inclus sau upsell?
7. Monitoring attach offer după fiecare report?
8. Organization/team accounts în MVP sau post-MVP?

## Data & retention

1. Retention per source snapshot class?
2. Retention oficial documents?
3. Retention mandates/consents?
4. Ce date pot fi șterse la cererea utilizatorului vs legal retention?
5. Păstrăm raw provider responses integral sau minimizate pentru anumite providers?

## Engineering

1. UUID vs ULID vs bigint pentru primary keys?
2. Laravel version baseline la bootstrap?
3. Queue worker management/deployment platform?
4. PDF renderer choice?
5. Payment adapter contract?
6. Object-storage provider/environment?
7. Observability stack?
8. Admin UI: custom Livewire vs package?

## Status labels

Folosiți în issue/notes:

- `OPEN`
- `RESEARCHING`
- `WAITING_PROVIDER`
- `DECISION_REQUIRED`
- `RESOLVED`
