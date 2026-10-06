# Systemarkitektur

## Formål

ReportGuard hjælper medarbejdere med at oprette ensartede leverandørmails fra
servicerapporter og advarer, når en rapport muligvis allerede er sendt.

Den første version forbereder en mail, men brugeren godkender og sender den selv.

## Teknologistak

| Område | Valg |
| --- | --- |
| Sprog | TypeScript |
| Dashboard | React og Vite |
| Outlook-tilføjelse | React og Office.js |
| API | NestJS og REST |
| Database | PostgreSQL med Prisma |
| Login | Microsoft Entra ID |
| Outlook-integration | Microsoft Graph |
| Filer | Azure Blob Storage |
| Drift | Docker, Azure og GitHub Actions |

## Systemdiagram

```mermaid
flowchart LR
    User[Medarbejder] --> Addin[Outlook-tilføjelse]
    User --> Dashboard[Webdashboard]
    Addin --> API[ReportGuard API]
    Dashboard --> API
    API --> DB[(PostgreSQL)]
    API --> Files[(Azure Blob Storage)]
    API --> Graph[Microsoft Graph]
    Graph --> Outlook[Microsoft Outlook]
```

## Arkitekturprincipper

1. Løsningen starter som en modulær monolit, ikke som microservices.
2. Backend er den eneste komponent med direkte databaseadgang.
3. Afsendelser registreres i ReportGuards egen revisionslog.
4. Outlook-mails sendes ikke automatisk i den første version.
5. Microsoft-rettigheder begrænses til det mindst nødvendige.
6. Hemmeligheder og adgangstokens må aldrig gemmes i repositoryet.

## Første produktflow

```mermaid
sequenceDiagram
    actor U as Medarbejder
    participant O as Outlook-tilføjelse
    participant A as ReportGuard API
    participant D as PostgreSQL
    participant G as Microsoft Graph

    U->>O: Vælger rapport og leverandør
    O->>A: Hent skabelon og rapportdata
    A->>D: Læs rapport, leverandør og historik
    D-->>A: Data og tidligere afsendelser
    A-->>O: Udfyldt mail og eventuelle advarsler
    U->>O: Gennemser og godkender mail
    O->>G: Opret eller opdater kladde
    U->>G: Sender mailen
    O->>A: Registrer afsendelsen
    A->>D: Gem revisionshændelse
```
