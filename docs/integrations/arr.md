# ARR Integration

Status: **CONDITIONAL**

## Scope

ARR is the target source for transport-operator verification.

Potential capabilities:

```text
arr.carrier_license
arr.carrier_vehicles
arr.transport_manager
```

## Current constraint

Public real-time verification exists, but a stable commercial API/feed has not yet been confirmed. EU-VERIFIC must not build the transport vertical on unsupported scraping.

## Intended product

`EV-TRANS-001` Verificare transportator should eventually combine:

- company identity;
- license type/state;
- validity;
- number of conforming copies/vehicles where available;
- transport manager where available;
- ANAF fiscal/financial checks;
- Justice litigation checks.

## Proposed normalized model

```text
carrier_id
company_id
license_type
license_number nullable
status
valid_from nullable
valid_until nullable
vehicle_count nullable
conforming_copy_count nullable
transport_manager nullable
source_snapshot_id
```

## Activation gate

Do not mark this provider `READY` until we have at least one of:

- official API;
- official web service;
- machine-readable feed with compatible reuse conditions;
- explicit written authorization for automated access.

## Open questions

- API/feed availability;
- commercial reuse rights;
- CUI as stable lookup key;
- rate limits;
- update frequency;
- historical license status;
- access to vehicle/copy counts.
