# raceday-part-3
# RaceDay - Part 3: MVC Web Application

ASP.NET Core 8 MVC front end for the RaceDay platform. It consumes the Part 2 REST API for **all** data (no database access or business logic in the MVC project). Images are uploaded to Azure Blob Storage **through the API**.

## Roles
- **Organiser**: dashboard, create/edit/delete events (with banner), categories, view enrolments, publish results.
- **Participant**: browse/filter upcoming events, event details, enrol with a category, my enrolments (colour-coded status), my results (e.g. 47th out of 312 finishers), profile + profile picture.
Add ApiClient service for MVC to API communication
Authentication state and role are kept in ASP.NET Core **session**; protected pages use a `SessionAuth` filter and redirect unauthenticated users to Login.

## Project structure
```
RaceDay.Mvc/
  Controllers/  Account, Home, Organiser, Participant
  Services/     ApiClient (single gateway to the API, friendly error handling)
  Filters/      SessionAuthAttribute (login + role checks)
  Models/       DTOs and view models
  Views/        Razor views per role + shared layout
  Dockerfile
docs/API-BlobStorage.md   (Azure Blob integration added to the API)
.github/workflows/ci.yml  (builds API + MVC + Docker image)
```

## Configuration
`RaceDay.Mvc/appsettings.json`:
```json
"ApiSettings": { "BaseUrl": "https://localhost:7001/" }
```
Set this to where your Part 2 API runs. Override with the env variable `ApiSettings__BaseUrl`.

## Run locally
1. Start the Part 2 API.
2. `cd RaceDay.Mvc && dotnet run`

## Run with Docker (single command)
From the repository root, with the API running on your machine at port 5000:
```
docker build -t raceday-mvc ./RaceDay.Mvc && docker run --rm -p 8080:8080 -e ApiSettings__BaseUrl=http://host.docker.internal:5000/ raceday-mvc
```
Open http://localhost:8080. (On Linux add `--add-host=host.docker.internal:host-gateway`.)
Implement SessionAuthorize filter for route protection
## Azure Blob Storage
See `docs/API-BlobStorage.md`. Banner and profile images are posted to the API, stored in Blob Storage, and the returned URL is displayed on the event detail page / profile.
Final testing and bug fixes for event management flow
Style frontend with Bootstrap and custom RaceDay theme
## Demo video
YouTube (unlisted): <ADD LINK HERE>
