# Login-provider

## Översikt

**LoginProvider** är ett front-end-projekt byggt med React och Vite för att hantera användarautentisering och UI-komponenter. Applikationen använder Storybook för att skapa och testa UI-komponenter i en isolerad miljö. Det finns stöd för olika inloggningsmetoder, inklusive JWT och lokal autentisering.

## Funktioner

- React och Vite som utvecklingsmiljö
- Storybook för UI-komponenttestning
- Olika inloggningsmetoder: JWT, lokal inloggning och mockade inloggningar
- Komponentbaserad arkitektur för återanvändbara UI-element som **LoginPage**, **UserAtom**, och **PasswordAtom**

## Krav

- Node.js (v18 eller senare)
- npm eller yarn

## Installation

1. Klona repot:
    ```bash
    git clone https://github.com/Ibrah111m/LoginProvider.git
    cd LoginProvider
    ```

2. Installera nödvändiga beroenden:
    ```bash
    npm install
    ```

## Skript

- Starta utvecklingsservern:
    ```bash
    npm run dev
    ```

- Bygg projektet:
    ```bash
    npm run build
    ```

- Starta Storybook för att testa UI-komponenter:
    ```bash
    npm run storybook
    ```

- Bygg Storybook:
    ```bash
    npm run build-storybook
    ```

- Lint din kod:
    ```bash
    npm run lint
    ```


## Login Context

```mermaid
stateDiagram-v2
    [*] --> Unauthenticated
    Unauthenticated --> Authenticating: Enter credentials
    Authenticating --> Authenticated: Credentials valid
    Authenticating --> Error: Credentials invalid
    Authenticated --> Unauthenticated: Logout
    Error --> Unauthenticated: Retry
    Error --> Authenticating: Enter credentials again
```

## Login Process

```mermaid
sequenceDiagram
    participant User
    participant Login
    participant LoginProvider as Login Provider
    participant Backend
    participant Fetcher
    participant Viewer

    User->>+Login: Enter credentials
    Login->>+LoginProvider: login(username, password)
    LoginProvider->>+Backend: POST /login (credentials)
    Backend->>Backend: Validate credentials
    Backend->>Backend: Set AccessToken & RefreshToken
    Backend-->>-LoginProvider: Response (AccessToken/RefreshToken)
    LoginProvider-->>-Login: Update state (isLoggedIn=true)
    Login-->>User: Display logged in state

    User->>+Fetcher: Request data
    Fetcher->>+LoginProvider: secureCall(apiUrl, path)
    LoginProvider->>+Backend: GET /resource (AccessToken)
    Backend->>Backend: Validate AccessToken
    Backend->>Backend: Refresh AccessToken if necessary
    Backend-->>-LoginProvider: Response (data)
    LoginProvider-->>-Fetcher: Update state (data)
    Fetcher->>+Viewer: Pass data
    Viewer-->>-User: Display data
```

# LoginProvider

## App Flow Diagram

![App Flow Diagram](./assets/diagram.png)

## Komponenter i Storybook

### Följande komponenter kan testas i Storybook:

- **LoginPage.stories.jsx** – Testar `LoginPage`-komponenten
- **UserAtom.stories.jsx** – Testar `UserAtom`-komponenten
- **PasswordAtom.stories.jsx** – Testar `PasswordAtom`-komponenten
- **LoginOrganism.stories.jsx** – Testar en organism med flera inloggningskomponenter
- **ProfileOrganism.stories.jsx** – Testar profilkomponenten

## Licens

Detta projekt är licensierat under MIT-licensen.
