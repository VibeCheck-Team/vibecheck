VibeCheck

VibeCheck är en fullstack-webbapplikation för att lära sig svenska slangord på ett interaktivt och engagerande sätt. 
Applikationen kombinerar quiz, en digital ordbok och tävlingsmoment för att göra inlärningen både lärorik och underhållande.

Användare kan skapa ett konto, genomföra quiz med olika svårighetsgrader, utforska svenska slangord,
följa sin utveckling och jämföra sina resultat med andra användare. 
Applikationen innehåller även ett demotest som kan genomföras utan inloggning.

**Liveversion**

VibeCheck är driftsatt på Microsoft Azure och kan användas direkt i webbläsaren.

https://vibecheck-api-gth3dudcbhaucgck.swedencentral-01.azurewebsites.net/

**Funktioner**

- Demotest som kan genomföras utan inloggning
- Registrering och inloggning med JWT-autentisering
- Quiz med olika frågetyper och svårighetsgrader
- Upplåsning av nya quiznivåer baserat på användarens resultat
- Digital ordbok med svenska slangord
- Sökning, filtrering och detaljerad information om ord
- Möjlighet att hissa och dissa slangord
- Profilsida med statistik och historik över genomförda quiz
- Möjlighet att ändra användarnamn och lösenord
- Rankat quiz med topplistor
- Administrationsgränssnitt för hantering av ord
- Responsiv design anpassad för datorer och mobiltelefoner

**Tekniker**

### Frontend

- React 19
- Vite
- React Router
- JavaScript
- CSS
- Playwright och Playwright-BDD

### Backend

- ASP.NET Core med .NET 10
- Entity Framework Core
- ASP.NET Core Identity
- JWT-autentisering
- REST API
- OpenAPI

### Databas och driftsättning

- Microsoft SQL Server
- Microsoft Azure
- Git och GitHub

 **Projektstruktur**
vibecheck/
├── frontend/                    React-applikationen
│   ├── src/
│   │   ├── api/                 API-anrop
│   │   ├── components/          Återanvändbara komponenter
│   │   ├── pages/               Applikationens sidor
│   │   └── styles/              CSS och gemensam styling
│   └── tests/                   End-to-end-tester
│
├── backend/
│   ├── VibeCheck.Api/           Controllers, services, DTO:er och API
│   ├── VibeCheck.Data/          Databasmodeller, DbContext och migrations
│   └── tests/                   Kontroller för seedning och quizlogik
│
└── README.md

**Starta projektet lokalt**

Förutsättningar

Följande behöver vara installerat:

.NET 10 SDK

Node.js och npm

SQL Server LocalDB eller en annan SQL Server-instans

Git

1. Klona projektet

git clone LÄGG-IN-REPOSITORY-LÄNKEN-HÄR
cd vibecheck

2. Konfigurera backend

Projektet använder JWT för autentisering. Skapa en lokal hemlig nyckel med minst 32 tecken:

dotnet user-secrets set "Jwt:Key" "en-hemlig-nyckel-med-minst-32-tecken" --project backend/VibeCheck.Api

Starta backend:

dotnet run --project backend/VibeCheck.Api

API startar normalt på:

https://localhost:7226

Databasmigrationer och seedning körs automatiskt när backend startas.

3. Konfigurera frontend

Skapa filen frontend/.env.local och lägg till följande:

VITE_API_URL=https://localhost:7226

Installera projektets paket och starta frontend:

cd frontend
npm install
npm run dev

Frontend startar normalt på:

http://localhost:5173

Öppna adressen i webbläsaren för att använda applikationen.

**Testning**

Frontend

Kontrollera kodkvaliteten med ESLint:

cd frontend
npm run lint

Installera Playwrights webbläsare första gången testerna körs:

npx playwright install

Kör applikationens end-to-end- och BDD-tester:

npm run test:e2e

Tester kan även köras i Playwrights grafiska gränssnitt:

npm run test:e2e:ui

Backend

Kontrollera databasens seedning:

dotnet run --project backend/tests/VibeCheck.SeedChecks

Kontrollera quizlogiken:

dotnet run --project backend/tests/VibeCheck.QuizChecks

**Testkonton**

Vid lokal utveckling skapas testdata och ett vanligt användarkonto genom projektets seedning.

Administratörskontot konfigureras separat genom lokala hemligheter eller miljövariabler. 
Inga administratörsuppgifter eller andra känsliga inloggningsuppgifter ska sparas i GitHub-repositoryt.

I den driftsatta versionen kan besökare registrera ett eget användarkonto för att testa applikationens funktioner.


**Databasmigrationer**

Projektet använder Entity Framework Core-migrationer för att hantera förändringar i databasens struktur.

Skapa en ny migration från projektets rot:

dotnet ef migrations add MigrationName --project backend/VibeCheck.Data --startup-project backend/VibeCheck.Api

Använd ett beskrivande namn som förklarar databasändringen, exempelvis:

dotnet ef migrations add AddQuizCategories --project backend/VibeCheck.Data --startup-project backend/VibeCheck.Api

Migrationen ska sparas tillsammans med de modelländringar som den tillhör.

Databasen uppdateras automatiskt med tillgängliga migrationer när backend startas:

dotnet run --project backend/VibeCheck.Api

Skapa inte flera parallella migrationer för samma databasändring. 
Kontrollera alltid befintliga migrationer och hämta den senaste versionen av projektet innan en ny migration skapas.




