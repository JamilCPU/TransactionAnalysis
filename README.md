# TransactionAnalysis

A full-stack demo banking application that simulates core financial operations: user registration, secure login, account creation, deposits, withdrawals, transfers and transaction history. It was built to practise layered backend design (controllers, services, repositories, EF Core migrations) alongside a typed React front end. It uses a local SQLite database and holds no real financial data.

## Stack

- Backend: ASP.NET Core Web API on .NET 9, Entity Framework Core 9 with SQLite and migrations, JWT bearer authentication, DotNetEnv for `.env` loading
- Frontend: React 19, TypeScript, Vite, Tailwind CSS, React Router, TanStack Query, Axios
- Tests: xUnit with `Microsoft.AspNetCore.Mvc.Testing` integration tests (`backend/test`)

## Running

You need the .NET 9 SDK and Node.js. Use two terminals from the repository root.

Backend (applies migrations and creates `backend/bank.db` on first start):

```bash
cd backend
dotnet run
```

Frontend:

```bash
cd frontend
npm install
npm run dev
```

Create `frontend/.env` so the app knows where the API lives:

```
VITE_API_BASE_URL=http://localhost:5000
VITE_FRONTEND_URL=http://localhost:5173
```

The backend allows CORS from `http://localhost:5173`. JWT settings come from the `Jwt` section of `backend/appsettings.json`; the key there is a placeholder, so override it with a long random value through the `Jwt__Key` environment variable (or a git-ignored `.env`) for anything beyond local use. `EnableDetailedLogging` in `.env` toggles verbose logging.

Run the tests:

```bash
cd backend/test
dotnet test
```

## API overview

| Area | Routes |
|------|--------|
| Auth | `POST /api/auth/register`, `POST /api/auth/login` |
| Users | `/api/users` (create, update, delete, get by id, info by username, list) |
| Accounts | `/api/accounts` (new, get, list, delete, `{id}/deposit`, `{id}/withdraw`, `transfer`) |
| Transactions | `/api/transactions` (new, get, list, delete, per-account history) |

## Project layout

```
backend/
  api/          controllers, DTOs, options
  service/      business logic (interfaces + impl)
  repository/   data access (interfaces + impl)
  domain/ data/ domain models, EF Core context and entities
  Migrations/   EF Core migrations
  test/         integration tests
frontend/src/   pages, components, contexts, API service
Bank.sln
```
