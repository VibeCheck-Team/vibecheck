# VibeCheck

VibeCheck är en fullstack-webbapplikation för att lära sig svenska slangord på ett interaktivt och engagerande sätt. Applikationen kombinerar quiz, en digital ordbok och tävlingsmoment för att göra inlärningen både lärorik och underhållande.

Användare kan skapa ett konto, genomföra quiz med olika svårighetsgrader, utforska svenska slangord, följa sin utveckling och jämföra sina resultat med andra. Applikationen innehåller även ett demotest som kan genomföras utan inloggning.

## Liveversion

Applikationen är driftsatt på Microsoft Azure och kan användas direkt i webbläsaren:

**https://vibecheck-api-gth3dudcbhaucgck.swedencentral-01.azurewebsites.net/**

> Miljön körs på Azures gratisnivå. Både webbtjänsten och databasen pausar vid inaktivitet, så första besöket efter en paus kan ta upp till en minut.

## Funktioner

| Område | Funktionalitet |
| --- | --- |
| Konto | Registrering och inloggning med JWT-autentisering, byte av användarnamn och lösenord |
| Quiz | Flera frågetyper och svårighetsgrader, upplåsning av nya nivåer baserat på resultat |
| Demo | Demotest som kan genomföras utan inloggning |
| Ordbok | Svenska slangord med sökning, filtrering och detaljvy |
| Community | Möjlighet att hissa och dissa slangord |
| Profil | Statistik och historik över genomförda quiz |
| Tävling | Rankat quiz med topplistor |
| Administration | Gränssnitt för hantering av ord |
| Design | Responsiv layout anpassad för både dator och mobil |

## Tekniker

**Frontend**

React 19 · Vite · React Router · JavaScript · CSS · Playwright och Playwright-BDD

**Backend**

ASP.NET Core med .NET 10 · Entity Framework Core · ASP.NET Core Identity · JWT-autentisering · REST API · OpenAPI

**Databas och driftsättning**

Microsoft SQL Server · Microsoft Azure · Git och GitHub

## Projektstruktur

```
vibecheck/
├── frontend/                       React-applikationen
│   ├── src/
│   │   ├── api/                    API-anrop
│   │   ├── components/             Återanvändbara komponenter
│   │   ├── pages/                  Applikationens sidor
│   │   └── styles/                 CSS och gemensam styling
│   └── features/                   Gherkin-scenarier och stegdefinitioner
│
├── backend/
│   ├── VibeCheck.Api/              Controllers, services, DTO:er och API
│   ├── VibeCheck.Data/             Databasmodeller, DbContext och migrationer
│   └── tests/                      Kontroller för seedning och quizlogik
│
└── README.md
```

## Kom igång lokalt

### Förutsättningar

- .NET 10 SDK
- Node.js och npm
- SQL Server LocalDB eller en annan SQL Server-instans
- Git

### 1. Klona projektet

```bash
git clone https://github.com/VibeCheck-Team/vibecheck.git
cd vibecheck
```

### 2. Konfigurera backend

Känsliga värden läses från konfiguration och ligger aldrig i källkoden. Sätt dem en gång med user-secrets:

```bash
cd backend/VibeCheck.Api

# Nyckel för JWT-signering, minst 32 tecken
dotnet user-secrets set "Jwt:Key" "en-hemlig-nyckel-med-minst-32-tecken"

# Adminkonto som skapas vid seedning
dotnet user-secrets set "Admin:Username" "admin"
dotnet user-secrets set "Admin:Email" "admin@vibecheck.local"
dotnet user-secrets set "Admin:Password" "Admin123!"
```

> Utan `Admin:Password` skapas ingen administratör. Adminsidorna blir då otillgängliga nästa gång databasen byggs om.

Starta backend från projektets rot:

```bash
dotnet run --project backend/VibeCheck.Api
```

API:et startar normalt på `https://localhost:7226`. Databasmigrationer och seedning körs automatiskt vid uppstart.

### 3. Konfigurera frontend

Skapa filen `frontend/.env.local`:

```
VITE_API_URL=https://localhost:7226
```

Installera paketen och starta utvecklingsservern:

```bash
cd frontend
npm install
npm run dev
```

Frontend startar normalt på `http://localhost:5173`. Öppna adressen i webbläsaren för att använda applikationen.

## Testning

### Frontend

```bash
cd frontend

# Kodkvalitet
npm run lint

# Playwrights webbläsare, behövs bara första gången
npx playwright install

# End-to-end- och BDD-tester
npm run test:e2e

# Samma tester i Playwrights grafiska gränssnitt
npm run test:e2e:ui
```

Scenarierna ligger som Gherkin i `frontend/features/` med tillhörande stegdefinitioner i `frontend/features/steps/`. Backend behöver vara igång när testerna körs.

### Backend

```bash
# Kontrollera databasens seedning
dotnet run --project backend/tests/VibeCheck.SeedChecks

# Kontrollera quizlogiken
dotnet run --project backend/tests/VibeCheck.QuizChecks
```

## Testkonton

Vid lokal utveckling skapas testdata och ett vanligt användarkonto genom projektets seedning. Administratörskontot konfigureras separat via user-secrets lokalt och miljövariabler i Azure.

Inga administratörsuppgifter eller andra känsliga inloggningsuppgifter sparas i repositoryt.

I den driftsatta versionen kan besökare registrera ett eget konto för att testa applikationens funktioner.

## Databasmigrationer

Projektet använder Entity Framework Core-migrationer för att hantera förändringar i databasens struktur.

Skapa en ny migration från projektets rot:

```bash
dotnet ef migrations add AddQuizCategories \
  --project backend/VibeCheck.Data \
  --startup-project backend/VibeCheck.Api
```

Använd ett beskrivande namn som förklarar vad databasändringen gör, och committa migrationen tillsammans med de modelländringar den hör till.

Databasen uppdateras automatiskt med tillgängliga migrationer när backend startas:

```bash
dotnet run --project backend/VibeCheck.Api
```

> Skapa inte flera parallella migrationer för samma databasändring. Hämta alltid senaste versionen av projektet och kontrollera befintliga migrationer innan en ny skapas.

## Driftsättning

Driftsättningen är automatiserad med GitHub Actions. Varje merge till `main` bygger frontend och API, paketerar dem tillsammans och publicerar till Azure App Service.

Konfiguration i molnet sätts som app settings i Azure, aldrig i källkoden:

| Inställning | Beskrivning |
| --- | --- |
| `ConnectionStrings__DefaultConnection` | Anslutning till Azure SQL |
| `Jwt__Key` | Nyckel för JWT-signering |
| `Admin__Username`, `Admin__Email`, `Admin__Password` | Administratörskontot |
| `ASPNETCORE_FORWARDEDHEADERS_ENABLED` | Krävs bakom Azures proxy |

Demoanvändare och exempeldata seedas endast i utvecklingsmiljö och finns inte i den driftsatta databasen.
