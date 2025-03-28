# Keycloak Integration with Angular & .NET API

## Overview

This project demonstrates how to integrate Keycloak authentication and authorization into an Angular frontend and a .NET API backend. Keycloak is an open-source identity and access management solution that provides authentication, authorization, and user management.

This project is detailed guide is available on Notion: [Keycloak Guide](https://www.notion.so/Keycloak-1c3b6bc7b65380d1a302fd6eaf50ce43?pvs=4).

## Tech Stack

- **Frontend**: Angular 15+
- **Backend**: .NET 7+ (ASP.NET Web API)
- **Authentication**: Keycloak
- **Database**: SQL Server (Optional for user persistence)

## Prerequisites

Before running the project, ensure you have the following installed:

- [Node.js](https://nodejs.org/) (LTS version recommended)
- [Angular CLI](https://angular.io/cli)
- [.NET SDK](https://dotnet.microsoft.com/)
- [Docker](https://www.docker.com/) (for running Keycloak instance)

## Setup Instructions

### 1. Running Keycloak

You can start Keycloak using Docker:

```sh
docker run -p 8080:8080 \
    -e KEYCLOAK_ADMIN=admin \
    -e KEYCLOAK_ADMIN_PASSWORD=admin \
    quay.io/keycloak/keycloak:latest \
    start-dev
```

- Open [http://localhost:8080](http://localhost:8080)
- Log in with `admin/admin`
- Create a new **Realm** (e.g., `myrealm`)
- Configure a **Client** for Angular and another one for .NET API

### 2. Setting up the Angular Frontend

1. Navigate to the Angular project directory:
   ```sh
   cd angular-app
   ```
2. Install dependencies:
   ```sh
   npm install
   ```
3. Configure Keycloak in `src/environments/environment.ts`:
   ```typescript
   export const environment = {
     production: false,
     keycloak: {
       url: "http://localhost:8080",
       realm: "myrealm",
       clientId: "angular-client",
     },
   };
   ```
4. Run the Angular application:
   ```sh
   ng serve --open
   ```

### 3. Setting up the .NET API Backend

1. Navigate to the .NET API project directory:
   ```sh
   cd dotnet-api
   ```
2. Install dependencies and build the project:
   ```sh
   dotnet restore
   dotnet build
   ```
3. Configure Keycloak authentication in `appsettings.json`:
   ```json
   "Jwt": {
     "Authority": "http://localhost:8080/realms/myrealm",
     "Audience": "dotnet-api-client"
   }
   ```
4. Run the API:
   ```sh
   dotnet run
   ```

## Usage

1. Open the Angular app in the browser.
2. Log in using Keycloak credentials.
3. The Angular app will fetch a JWT token and use it to communicate with the .NET API.
4. The .NET API will validate the token before allowing access to protected endpoints.

## Notes

- Ensure Keycloak is running before starting Angular or .NET API.
- Update Keycloak configuration in Angular and .NET API based on your Keycloak setup.
- Use HTTPS in production for secure communication.

## License

This project is open-source and available under the [MIT License](LICENSE).
