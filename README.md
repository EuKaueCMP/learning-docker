# learning-docker

Practical guide and implementation repository for containerizing ASP.NET Core applications with Docker.

## Description

learning-docker documents hands-on progress with Docker and containerization techniques. It features a complete multi-stage Dockerfile implementation containerizing the Royal Games backend ASP.NET Core Web API, demonstrating layer caching, SDK build stages, lightweight runtime separation, and container networking.

## Technologies

- **Containerization:** Docker
- **Base Images:**
  - `mcr.microsoft.com/dotnet/sdk:8.0` (Build stage)
  - `mcr.microsoft.com/dotnet/aspnet:8.0` (Runtime stage)
- **Backend Framework:** ASP.NET Core 8.0 Web API
- **Language:** C#

## Project Structure

```text
learning-docker/
├── RoyalGames/
│   ├── Applications/           # Business services and validation logic
│   ├── Contexts/               # Entity Framework database contexts
│   ├── Controllers/            # REST API endpoint controllers
│   ├── DTOs/                   # Data transfer objects
│   ├── Domains/                # Domain models and entities
│   ├── Interfaces/             # Service and repository contracts
│   ├── Repositories/           # Data access layer
│   ├── Dockerfile              # Multi-stage Docker build configuration
│   ├── Program.cs              # API initialization and DI configuration
│   └── RoyalGames.csproj
└── README.md
```

## Docker Architecture

The container build implements a **multi-stage build** pipeline to optimize image size and build caching:

1. **Build Stage (`mcr.microsoft.com/dotnet/sdk:8.0`):**
   - **Layer Caching:** Copies `RoyalGames.csproj` first and runs `dotnet restore` to cache NuGet package restoration layers independently of code edits.
   - **Release Compilation:** Copies project sources and compiles optimized release binaries via `dotnet publish -c Release -o /app_publish`.

2. **Runtime Stage (`mcr.microsoft.com/dotnet/aspnet:8.0`):**
   - Strips away build tools and SDK binaries, retaining only the minimal ASP.NET Core runtime.
   - Copies compiled artifacts from the build stage (`COPY --from=build`).
   - Exposes container port `8080`.
   - Defines the container execution entrypoint (`dotnet RoyalGames.dll`).

## Setup & Execution

### Prerequisites
- [Docker Engine](https://docs.docker.com/engine/install/) or Docker Desktop installed and running
- [Git](https://git-scm.com/)

### Building the Docker Image
1. Clone the repository:
```bash
git clone https://github.com/EuKaueCMP/learning-docker.git
```

2. Navigate to the project root:
```bash
cd learning-docker
```

3. Build the Docker image using the Dockerfile inside `RoyalGames`:
```bash
docker build -t royalgames-api -f RoyalGames/Dockerfile RoyalGames/
```

### Running the Container
Execute the container, mapping host port `8080` to container port `8080`:
```bash
docker run -d -p 8080:8080 --name royalgames-container royalgames-api
```

## Developer

**Kauê Sérgio Campos**  
GitHub: [@EuKaueCMP](https://github.com/EuKaueCMP)
