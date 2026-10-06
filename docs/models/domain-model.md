# Domænemodel

Dette er den første konceptuelle model. Den er ikke endnu et endeligt
databaseskema.

```mermaid
erDiagram
    ORGANIZATION ||--o{ USER : has
    ORGANIZATION ||--o{ SUPPLIER : manages
    ORGANIZATION ||--o{ SERVICE_REPORT : owns
    SUPPLIER ||--o{ SUPPLIER_CONTACT : has
    SUPPLIER ||--o{ EMAIL_TEMPLATE : uses
    SERVICE_REPORT ||--o{ REPORT_ATTACHMENT : contains
    SERVICE_REPORT ||--o{ EMAIL_DRAFT : creates
    EMAIL_TEMPLATE ||--o{ EMAIL_DRAFT : formats
    USER ||--o{ EMAIL_DRAFT : prepares
    EMAIL_DRAFT ||--o| EMAIL_DELIVERY : becomes
    EMAIL_DELIVERY ||--o{ DUPLICATE_WARNING : can_trigger
    USER ||--o{ AUDIT_EVENT : performs
    SERVICE_REPORT ||--o{ AUDIT_EVENT : records

    SERVICE_REPORT {
        uuid id
        string report_number
        string status
        datetime service_date
        datetime updated_at
    }

    EMAIL_TEMPLATE {
        uuid id
        uuid supplier_id
        string name
        string subject_template
        text body_template
        int version
        boolean active
    }

    EMAIL_DELIVERY {
        uuid id
        uuid service_report_id
        uuid supplier_id
        string recipient_fingerprint
        string attachment_fingerprint
        datetime sent_at
    }

    AUDIT_EVENT {
        uuid id
        string event_type
        json metadata
        datetime occurred_at
    }
```

## Regler for dubletkontrol

En mail markeres som en mulig dublet, hvis mindst én af følgende kombinationer
allerede findes:

- Samme rapportnummer og leverandør.
- Samme rapportnummer og modtager.
- Samme vedhæftningsfingeraftryk og modtager inden for en fastsat periode.

En advarsel stopper ikke permanent afsendelsen. Brugeren kan fortsætte med en
begrundelse, som gemmes i revisionsloggen.
