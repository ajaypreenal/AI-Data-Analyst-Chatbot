# AAROH Connect AI

An AI-powered WhatsApp Customer Engagement Platform for businesses.

## Features

- **Dashboard**: Real-time analytics, messages sent, pending approvals.
- **Customer Management**: Store customer details and track engagement.
- **WhatsApp Webhook**: Send and receive messages via the official WhatsApp Business Cloud API.
- **AI Reply Assistant**: Contextual AI replies generated using OpenAI (GPT-4o) and Langchain.
- **Human-in-the-Loop**: Default "Human Approval" mode for AI replies, with an Auto Mode toggle.
- **Knowledge Base (RAG)**: Upload documents to train your AI. (Architecture supported).

## Tech Stack

- **Frontend**: Next.js 15, TailwindCSS, shadcn/ui, Framer Motion
- **Backend**: NestJS, Prisma ORM, Redis (Optional), LangChain
- **Database**: PostgreSQL
- **AI**: OpenAI API

## Local Development

### 1. Database Setup
You will need a PostgreSQL database. You can run one locally using Docker:
```bash
docker run --name aaroh-postgres -e POSTGRES_PASSWORD=mysecretpassword -p 5432:5432 -d postgres
```

### 2. Backend Setup
1. Navigate to the backend directory:
   ```bash
   cd backend
   ```
2. Install dependencies:
   ```bash
   npm install
   ```
3. Copy the `.env` file (create it if missing) and set:
   ```env
   DATABASE_URL="postgresql://postgres:mysecretpassword@localhost:5432/aaroh?schema=public"
   OPENAI_API_KEY="sk-your-openai-api-key"
   WHATSAPP_VERIFY_TOKEN="aaroh_connect_secret"
   ```
4. Push Prisma schema:
   ```bash
   npx prisma db push
   ```
5. Start the backend:
   ```bash
   npm run start:dev
   ```

### 3. Frontend Setup
1. Navigate to the frontend directory:
   ```bash
   cd frontend
   ```
2. Install dependencies:
   ```bash
   npm install
   ```
3. Start the Next.js server:
   ```bash
   npm run dev
   ```
   Access the app at `http://localhost:3000`.

## Deployment Guide

### Frontend (Vercel)
1. Push your repository to GitHub.
2. Import the project in Vercel.
3. Set the Root Directory to `frontend`.
4. Deploy.

### Backend (Railway)
1. In Railway, click **New Project** -> Deploy from GitHub.
2. Select this repository and set the Root Directory to `backend`.
3. Add a PostgreSQL database plugin in Railway.
4. Set the `DATABASE_URL` environment variable.
5. In the Build settings, set the build command to `npm run build` and start command to `npm run start:prod`.

## Docker Configuration (Backend)
If you prefer to deploy the backend via Docker, a `Dockerfile` is provided in the `backend` directory.
```dockerfile
FROM node:20-alpine AS builder
WORKDIR /app
COPY package*.json ./
COPY prisma ./prisma/
RUN npm install
COPY . .
RUN npx prisma generate
RUN npm run build

FROM node:20-alpine
WORKDIR /app
COPY --from=builder /app/node_modules ./node_modules
COPY --from=builder /app/package*.json ./
COPY --from=builder /app/dist ./dist
COPY --from=builder /app/prisma ./prisma
EXPOSE 3000
CMD [ "npm", "run", "start:prod" ]
```

## Testing Strategy
- **Backend**: We use `Jest` for unit and E2E testing. Run `npm run test` or `npm run test:e2e` inside the `backend` directory.
- **Frontend**: The UI can be tested manually or by adding `Cypress` / `Playwright`.

## Seed Data
To populate your database with dummy customers, you can run a script or manually insert rows using Prisma Studio:
```bash
npx prisma studio
```
