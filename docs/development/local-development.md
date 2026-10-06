# Lokal udvikling

## Forudsætninger

- Node.js 22 eller nyere.
- pnpm-versionen angivet i rodprojektets `packageManager`-felt.

## Arbejdsområder

| Mappe | Ansvar |
| --- | --- |
| `apps/dashboard` | Webdashboardet |
| `apps/outlook-addin` | Outlook-tilføjelsen |
| `services/api` | Backend-API'et |
| `packages/shared` | Delte TypeScript-typer og valideringsregler |

## Grundkommandoer

```bash
pnpm install
pnpm typecheck
pnpm test
```

Applikationerne tilføjes i separate, små branches. Denne grundstruktur indeholder
med vilje ingen produktfunktionalitet.
