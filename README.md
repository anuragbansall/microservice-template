# Microservice Template (Node.js + Express + Mongoose)

A minimal, production-minded microservice starter with clear folder structure, environment-driven config, health check, and testing via Jest + Supertest + MongoDB Memory Server.

## Use This as a GitHub Template

1. On GitHub, click "Use this template" on this repo.
2. Name your new repository and create it in your account/org.
3. Clone your new repository locally.
4. Follow the setup steps below.

## Prerequisites

- Node.js 18+ and npm
- A MongoDB connection string (local, Docker, or Atlas)

## Quick Start

```bash
# Install dependencies
npm install

# (Optional) Install nodemon for dev reloads
npm i -D nodemon

# Create a .env file (see below)

# Start in dev (watches server.js)
npm run dev

# Start in production mode
npm start

# Run tests
npm test
```

## Environment Variables

This template loads config from `.env` via `src/config/index.js`. At minimum, set `MONGO_URI`.

Example `.env`:

```
# Server
PORT=3000

# Database
MONGO_URI=mongodb://localhost:27017/microservice

# Security
JWT_SECRET=replace-with-a-strong-secret
```

Notes:

- If `MONGO_URI` is missing, the server exits with an error at startup.
- `PORT` defaults to 3000 if not set.

## Scripts

- `npm start`: Runs the service with Node (`server.js`).
- `npm run dev`: Runs with `nodemon` watching `server.js` (install `nodemon` if missing).
- `npm test`: Runs Jest in-band with Node ESM support.

## Project Structure

```
├─ jest.config.cjs
├─ package.json
├─ server.js                # Entry: DB connect, start app
├─ src/
│  ├─ app.js               # Express app, middlewares, routes
│  ├─ config/
│  │  └─ index.js          # dotenv + exported config
│  ├─ db/
│  │  └─ index.js          # Mongoose connection helper
│  ├─ middlewares/         # Custom middlewares (e.g., auth)
│  ├─ models/              # Mongoose models
│  ├─ routes/              # Route modules
│  ├─ utils/               # Helpers/utilities
│  └─ validations/         # Request validations
├─ tests/
│  ├─ service.health.test.js
│  └─ setup.js             # In-memory Mongo and cleanup
```

## Health Check

A basic health endpoint is included:

- `GET /api/health` → `{ status: "OK", message: "service is running" }`

## Add Routes

Create route files under `src/routes` and mount in `src/app.js`.

Example route module: `src/routes/example.js`

```js
import { Router } from "express";
const router = Router();

router.get("/example", (req, res) => {
  res.json({ ok: true });
});

export default router;
```

Mount in `src/app.js`:

```js
import exampleRoutes from "./routes/example.js";
app.use("/api", exampleRoutes);
```

## Define Models

Place Mongoose models in `src/models` and use them in your routes/services.

Example model: `src/models/User.js`

```js
import mongoose from "mongoose";

const userSchema = new mongoose.Schema(
  {
    email: { type: String, required: true, unique: true },
    name: { type: String, required: true },
  },
  { timestamps: true },
);

export default mongoose.model("User", userSchema);
```

## Middlewares & Validations

- Add app-wide middlewares in `src/app.js` (e.g., `cookie-parser`, JSON, CORS).
- Add request validators (e.g., `express-validator`) in `src/validations`.

## Database Connection

- Startup connects to MongoDB via `src/db/index.js` using `config.MONGO_URI`.
- On success, logs `MongoDB connected successfully`.
- On failure, logs the error and exits.

## Testing

This template uses:

- Jest (`jest.config.cjs`)
- Supertest for HTTP testing
- `mongodb-memory-server` for isolated DB tests

Run:

```bash
npm test
```

Example health test in `tests/service.health.test.js` shows how to test the Express app without starting the server.

## Deployment Notes

- Ensure `MONGO_URI`, `PORT`, and `JWT_SECRET` are set in your environment.
- Use a process manager (PM2, Docker, or your platform) to run `node server.js`.

## Troubleshooting

- "MONGO_URI is not defined": Add `MONGO_URI` to `.env` or environment.
- `npm run dev` fails: Install `nodemon` (`npm i -D nodemon`).
- Port already in use: Change `PORT` or free the port.

## License

ISC
