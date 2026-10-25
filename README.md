# CoBuild

**Build together.**

CoBuild is a project-oriented social platform where people connect through ideas, collaborate on real projects, develop their skills, and earn reputation for their contributions.

Instead of focusing on ordinary social media posts, CoBuild focuses on what people create together. Whether you're developing software, writing a story, building a business, or working on a creative idea, CoBuild helps you find collaborators and turn your plans into progress.

## Table of Contents

- [Overview](#overview)
- [Core Features](#core-features)
- [How It Works](#how-it-works)
- [Project Lifecycle](#project-lifecycle)
- [Technology Stack](#technology-stack)
- [Architecture](#architecture)
- [Communication Protocol](#communication-protocol)
- [API Overview](#api-overview)
- [Getting Started](#getting-started)
- [Testing the API](#testing-the-api)
- [Android Configuration](#android-configuration)
- [Current Development Status](#current-development-status)
- [Roadmap](#roadmap)
- [Security](#security)
- [Contributing](#contributing)
- [License](#license)

## Overview

Traditional social networks focus on sharing content and following people. CoBuild takes a different approach by making projects the center of the experience.

Users can create projects, discover opportunities, join teams, complete assigned tasks, communicate with teammates, and showcase their achievements.

Each project provides a dedicated environment for collaboration, including roles, tasks, discussions, and project communication.

### Our Goals

- Connect people who want to build something together.
- Help users discover projects that match their interests and skills.
- Encourage meaningful collaboration and accountability.
- Recognize contributions through a reputation system.
- Give users a place to showcase their experience and completed projects.
- Make collaboration accessible to beginners and experienced creators alike.

## Core Features

The following describes the planned product experience. Availability depends on the current implementation.

### Projects

- Create projects with a name and description.
- Choose between public and private visibility.
- Specify required skills.
- Set a maximum team size.
- Discover projects that match your interests.
- Follow projects to keep up with their progress.
- View project information, members, roles, and status.

### Team Collaboration

- Join public projects or request access when approval is required.
- Invite users to private projects.
- Assign roles and responsibilities.
- Organize work into tasks.
- Assign each task to a specific member.
- Track progress and review completed tasks.
- Build a record of previous and completed projects.

### Social Features

- Publish project updates and posts.
- Review posts before they become visible to project members.
- Like and comment on supported content.
- Follow projects and discover related opportunities.
- Explore user profiles, skills, and previous projects.
- Receive notifications about relevant activity.

### Communication

- Communicate with project members through project chat.
- Send direct messages between two users.
- Share supported images, videos, and files.
- Keep conversations associated with the relevant project or recipient.

### Reputation and Skills

- Earn reputation through verified contributions.
- Display skills and project experience on your profile.
- Recognize helpful contributions and completed tasks.
- Build a portfolio based on real collaborative work.

Reputation calculations and task rewards are intended to be handled by the server to prevent clients from assigning themselves points.

### Discovery and Navigation

The planned Android application uses five main navigation destinations:

1. **Feed:** Discover relevant project activity.
2. **Search:** Find people and projects.
3. **Create Project:** Start a new collaboration.
4. **Notifications:** Review important updates.
5. **Profile:** Manage your skills and view your projects and reputation.

## How It Works

A typical CoBuild workflow looks like this:

1. A user creates a project and defines its visibility, required skills, and team size.
2. Other users discover the project through search or their feed.
3. Interested users join or request to join, depending on the project's rules.
4. The project creator organizes the team by assigning roles and tasks.
5. Members collaborate, communicate, and submit completed work.
6. Authorized project members review contributions according to project rules.
7. Verified contributions can affect the member's reputation.
8. Completed projects become part of the team's project history.

## Project Lifecycle

Projects use four planned statuses:

| Status | Description |
|---|---|
| `OPEN` | The project is accepting new members. |
| `IN_PROGRESS` | The project is underway and no longer accepts new members. |
| `COMPLETED` | The project has been completed. |
| `CANCELLED` | The project has been cancelled. |

Public projects can be discovered by users. Private projects are invitation-only.

The project creator manages the project within the platform's authorization rules.

## Technology Stack

CoBuild is being developed with a native Android client and a Python backend.

| Component | Technology |
|---|---|
| Android application | Java |
| Android UI | XML layouts and Android views |
| Backend | Python |
| API framework | FastAPI |
| Communication | REST API |
| Data format | JSON |
| Development server | Uvicorn |
| Initial testing | FastAPI Swagger UI |
| Planned persistent storage | SQLite for the prototype, with PostgreSQL as a possible later option |

The client and server are separate components. This allows the Android application to communicate with the backend through a defined API rather than handling shared data independently on each device.

## Architecture

### Backend

The planned backend architecture separates responsibilities into layers:

```text
Android Client
      |
      v
REST API
      |
      v
Authentication and Validation
      |
      v
Controllers
      |
      v
Services
      |
      v
Repositories
      |
      v
Database
```

File uploads and media storage will use a separate storage service.

- **API layer:** Receives HTTP requests and returns structured responses.
- **Authentication and validation:** Checks credentials, permissions, and input.
- **Controllers:** Connect API endpoints to application logic.
- **Services:** Implement business rules, project workflows, and reputation calculations.
- **Repositories:** Handle database operations.
- **Database:** Stores persistent users, projects, tasks, and other application data.
- **File service:** Manages uploaded media and file metadata.

The current starter backend is a minimal prototype and does not yet implement all these layers.

### Android Client

The intended client architecture follows a separation-of-concerns approach:

```text
Activities and UI
       |
       v
ViewModels
       |
       v
Repositories
       |
       v
Network Layer
       |
       v
CoBuild REST API
```

This structure helps keep UI code separate from networking and business logic as the application grows.

## Communication Protocol

CoBuild uses a JSON-based REST API. The protocol is being developed under the name **CAP/1 (CoBuild Application Protocol, version 1)**.

### API conventions

- API paths use the `/api/v1/` prefix.
- Requests and responses use JSON.
- Resource identifiers use UUIDs.
- Timestamps use ISO 8601 format, preferably UTC.
- Production communication must use HTTPS.
- Authenticated endpoints will use Bearer access tokens.
- Responses include a request identifier and a consistent status structure.

### Successful response example

```json
{
  "requestId": "66edd6a4-bfec-4ec0-be57-8da2076911a6",
  "success": true,
  "statusCode": 201,
  "message": "Project created successfully.",
  "data": {
    "projectId": "783f0f3e-a083-44b7-8730-efe01b647a36",
    "name": "My First CoBuild Project",
    "description": "Testing the Android app connection",
    "visibility": "PUBLIC",
    "skills": ["Java", "Python"],
    "maxMembers": 10,
    "status": "OPEN",
    "createdAt": "2026-10-21T06:50:41.688478+00:00"
  }
}
```

This example demonstrates the format returned by the current project-creation prototype.

## API Overview

The current prototype exposes these endpoints:

| Method | Endpoint | Purpose |
|---|---|---|
| `GET` | `/api/v1/health` | Check server availability. |
| `POST` | `/api/v1/projects` | Validate and create a project response. |
| `GET` | `/docs` | Open interactive API documentation. |

The full API is planned to expand into the following modules:

| Module | Responsibility |
|---|---|
| `/auth` | Registration, login, and token management |
| `/users` | User profiles and skills |
| `/projects` | Project creation and management |
| `/members` | Project membership and roles |
| `/join-requests` | Requests to join projects |
| `/invitations` | Invitations to private projects |
| `/tasks` | Task assignment and completion |
| `/posts` | Project updates and posts |
| `/comments` | Comments and replies |
| `/chat` | Project messaging |
| `/conversations` | Direct-message conversations |
| `/notifications` | User notifications |
| `/follow` | Following projects |
| `/feed` | Personalized project activity |
| `/search` | People and project discovery |
| `/files` | Media and file uploads |
| `/settings` | User preferences |
| `/reputation` | Reputation records and calculations |

These modules represent the planned API surface, not a claim that every endpoint is already implemented.

## Getting Started

### Prerequisites

Install the following software:

- Python 3.8 or newer for the current starter code.
- Android Studio for Android development.
- An Android emulator or compatible Android device.
- Git, if you want to clone the repository.

The backend uses FastAPI and Uvicorn. A virtual environment is recommended to isolate Python dependencies.

### 1. Clone the repository

Replace the placeholder URL with the actual repository URL:

```bash
git clone <YOUR_REPOSITORY_URL>
cd CoBuild
```

Alternatively, download and extract the project files.

### 2. Open the backend directory

```bash
cd server
```

### 3. Create a virtual environment

On Windows Command Prompt:

```bat
py -m venv .venv
.venv\Scripts\activate.bat
```

On Windows PowerShell:

```powershell
py -m venv .venv
.\.venv\Scripts\Activate.ps1
```

On macOS or Linux:

```bash
python3 -m venv .venv
source .venv/bin/activate
```

### 4. Install dependencies

```bash
python -m pip install -r requirements.txt
```

### 5. Start the server

```bash
python -m uvicorn main:app --host 0.0.0.0 --port 8000 --reload
```

If the server starts successfully, the terminal should report that the application startup is complete.

Keep the terminal running while testing the application.

### 6. Verify the server

Open these addresses in your browser:

- Health check: `http://127.0.0.1:8000/api/v1/health`
- Interactive API documentation: `http://127.0.0.1:8000/docs`

The health endpoint should return a successful response, and the documentation page should display the available API endpoints.

## Testing the API

You can test project creation directly through FastAPI's interactive documentation.

1. Open `http://127.0.0.1:8000/docs`.
2. Expand `POST /api/v1/projects`.
3. Select **Try it out**.
4. Enter the following JSON request body.
5. Click **Execute**.

```json
{
  "name": "My First CoBuild Project",
  "description": "Testing the Android app connection",
  "visibility": "public",
  "maxMembers": 10,
  "skills": ["Java", "Python"]
}
```

The current prototype should return a response with status code `201` when the request is valid.

**Important:** The API currently generates and returns project data without persistent database storage. A successful response does not mean the project will still exist after the server restarts.

## Android Configuration

Open the Android project in Android Studio and run it on an emulator.

The starter application uses the following development API base URL:

```java
private static final String BASE_URL =
        "http://10.0.2.2:8000/api/v1";
```

`10.0.2.2` is the special address Android emulators use to access the host computer's loopback network.

For a physical Android device, use the computer's reachable local network IP address instead, with both devices connected to an appropriate network.

### Development networking notes

- Start the Python backend before testing the Android client.
- Keep the backend and client configured for the same API port.
- Ensure Windows Firewall allows the required local development traffic.
- The starter project may use cleartext HTTP for local testing only.
- Do not use the development HTTP configuration as a production deployment.

## Current Development Status

CoBuild is under active development.

The current starter project establishes the initial communication path between the Android Java application and the Python backend.

| Component | Status |
|---|---|
| FastAPI server startup | Working |
| Health-check endpoint | Working |
| Interactive API documentation | Working |
| Project-creation request validation | Tested |
| Structured project-creation response | Tested |
| Android-to-server connection | To be verified in the running emulator |
| Persistent database | Not implemented in the current starter |
| User registration and authentication | Planned |
| Project membership and invitations | Planned |
| Tasks and reputation | Planned |
| Project posts and chat | Planned |
| Production deployment and HTTPS | Planned |

The current backend is a starting point for development, not a production-ready social network.

## Roadmap

### Phase 1: Foundation
- [x] Create the initial Android and Python starter structure.
- [x] Start the FastAPI development server.
- [x] Implement a health-check endpoint.
- [x] Validate project-creation requests.
- [x] Return a structured project-creation response.
- [x] Verify Android emulator connectivity.

### Phase 2: Persistent Data
- [ ] Add a database and repository layer.
- [ ] Persist projects across server restarts.
- [ ] Implement user and project data models.
- [ ] Add database migrations and validation.

### Phase 3: Authentication
- [ ] Implement registration and login.
- [ ] Add secure password hashing.
- [ ] Introduce access and refresh tokens.
- [ ] Enforce server-side authorization.

### Phase 4: Core Projects
- [ ] Build project discovery and search.
- [ ] Implement membership and join requests.
- [ ] Add invitations and project roles.
- [ ] Implement tasks and approval workflows.

### Phase 5: Social Collaboration
- [ ] Add project posts and comments.
- [ ] Implement project chat and direct messages.
- [ ] Add notifications and project following.
- [ ] Build profiles, skills, and reputation.

### Phase 6: Production Readiness
- [ ] Add HTTPS and production configuration.
- [ ] Implement rate limiting and stronger input validation.
- [ ] Add automated tests and error monitoring.
- [ ] Configure secure media storage.
- [ ] Improve performance, reliability, and deployment.

## Security

Security is an important part of CoBuild's planned architecture.

The production implementation should include:

- HTTPS for all client-server communication.
- Secure password hashing, such as Argon2id.
- Short-lived access tokens and protected refresh-token rotation.
- Server-side permission checks for every protected operation.
- Validation of user input and uploaded files.
- Rate limiting for authentication and sensitive endpoints.
- Protection against unauthorized task approvals and reputation manipulation.
- Secure storage of secrets and environment-specific configuration.
- Logging that avoids exposing passwords, tokens, or sensitive user data.

The current prototype does not yet implement the complete security architecture described above. It should only be used in a controlled development environment.

## Contributing

Contributions, suggestions, bug reports, and feature ideas are welcome.

To contribute:

1. Fork the repository.
2. Create a branch for your changes.
3. Make your changes and test them.
4. Commit your work with a descriptive message.
5. Open a pull request describing the changes.

For larger changes, consider opening an issue first to discuss the proposed implementation.

**CoBuild: Build together.**

Great projects are rarely built alone. CoBuild aims to make finding the right people, organizing the work, and recognizing real contributions part of one connected experience.
