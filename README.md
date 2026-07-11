# AI Email Writer

An AI-powered email reply generator built as a full-stack project with three integrated components: a Spring Boot backend, a React web app, and a Chrome extension that injects an "AI Reply" button directly into Gmail.

## Overview

This project lets us generate intelligent, context-aware email replies using Google's Gemini API. We can use it either through a standalone web interface or directly inside Gmail via a Chrome extension, both of which talk to the same Spring Boot backend.

## Architecture

```
┌─────────────────────┐        ┌──────────────────────┐        ┌────────────────────┐
│  email-writer-react │        │   email-writer-sb    │        │   Gemini API        │
│  (React + Vite +    │──────▶│   (Spring Boot        │──────▶│   (Google AI)        │
│   Material UI)       │        │    REST API)          │        │                      │
└─────────────────────┘        └──────────────────────┘        └────────────────────┘
                                        ▲
                                        │
                          ┌──────────────────────────┐
                          │   email-writer-ext        │
                          │   (Chrome Extension /      │
                          │    Manifest v3)            │
                          └──────────────────────────┘
```

- **Backend** (`email-writer-sb`) exposes a REST API that receives email content, forwards it to the Gemini API, and returns a generated reply.
- **Frontend** (`email-writer-react`) is a standalone web app where users can paste email content and get a generated reply in the browser.
- **Chrome Extension** (`email-writer-ext`) injects an "AI Reply" button into the Gmail compose/reply UI, letting users generate replies without leaving their inbox.

## Tech Stack

| Layer              | Technologies                                              |
|--------------------|-------------------------------------------------------------|
| Backend            | Java 17+, Spring Boot, Spring Web, Lombok, WebClient (Project Reactor) |
| AI Integration     | Google Gemini API                                          |
| Web Frontend       | React, Vite, Material UI                                    |
| Browser Extension  | Chrome Extension (Manifest v3), JavaScript, DOM manipulation |

## Project Structure

```
.
├── email-writer-sb/       # Spring Boot backend (REST API)
├── email-writer-react/    # React web app
└── email-writer-ext/      # Chrome extension for Gmail
```

## Getting Started

### Prerequisites

- Java 17 or higher
- Maven (or the included Maven wrapper)
- Node.js and npm/yarn
- A Google Gemini API key
- Google Chrome (for the extension)

### 1. Backend Setup (`email-writer-sb`)

```bash
cd email-writer-sb
```

Add your Gemini API key and endpoint to `src/main/resources/application.properties`:

```properties
gemini.api.url=YOUR_GEMINI_API_URL
gemini.api.key=YOUR_GEMINI_API_KEY
```

Run the application:

```bash
./mvnw spring-boot:run
```

The backend will start on `http://localhost:8080` by default.

### 2. Frontend Setup (`email-writer-react`)

```bash
cd email-writer-react
npm install
npm run dev
```

The web app will start on `http://localhost:5173` by default (or as configured by Vite). Make sure the backend URL used in the frontend's API calls matches where your Spring Boot server is running.

### 3. Chrome Extension Setup (`email-writer-ext`)

1. Open Chrome and navigate to `chrome://extensions`.
2. Enable **Developer mode** (top right).
3. Click **Load unpacked** and select the `email-writer-ext` folder.
4. Open Gmail — you should see an **AI Reply** button injected into the compose/reply UI.
5. Make sure the backend is running, since the extension calls it directly.

## Key Features

- Generate AI-powered email replies from a web interface or directly inside Gmail.
- Asynchronous, non-blocking backend calls to the Gemini API using WebClient and Project Reactor.
- Seamless Gmail integration via a Manifest v3 Chrome extension with content scripts.
- CORS-configured REST API supporting multiple client origins (web app and extension).
- Support tone/style customization for generated replies (formal, casual, concise, etc.).

## What I Learned

- Structuring a RESTful service in Spring Boot and handling asynchronous HTTP requests with WebClient and Project Reactor.
- Working with LLM APIs: prompt engineering and managing token-based responses.
- Building a Manifest v3 Chrome extension, including content scripts and DOM manipulation on third-party sites like Gmail.
- Full-stack integration across a Java backend, a standalone React frontend, and a browser extension, including CORS configuration.

## Future Improvements

- Add authentication/authorization for the API.
- Add unit and integration tests across all three modules.
- Deploy the backend and frontend to a cloud provider for public use.

