# Internship REST API

Beginner-friendly REST API using **Node.js, Express and SQLite**. Includes CRUD, pagination, filtering, validation, consistent JSON errors, seed records and automated validator tests.

> **Sample-data note:** Included records are fictional demo records; `example.com` application URLs are placeholders, not real vacancies. EdVyro supplies official [JSON seed records](https://edvyro.in/datasets/source-materials/internship-records-sample.json) and a [SQLite-compatible SQL file](https://edvyro.in/datasets/source-materials/internship-board-sample.sql). If the reviewer expects those exact records, replace `data/seed-internships.json` with the supplied JSON before seeding.

## Requirements
- Node.js 20 or newer
- npm

## Run locally
```bash
npm install
npm run seed
npm start
```
Open `http://localhost:3000`. Health check: `/api/health`; list internships: `/api/internships`.

Development mode: `npm run dev`. Tests: `npm test`.

SQLite database is automatically created at `data/internships.db`. The database file is ignored by Git; the seed command builds it from JSON. To reset, stop the server, delete `data/internships.db` (and optional `-wal`/`-shm` files), then run `npm run seed`.

## Data model
Each record contains `id`, `title`, `company`, `domain`, `location`, `work_mode`, `description`, `skills` (string array), `application_url`, `stipend`, `created_at`, and `updated_at`. Required fields: title, company, domain, location, work_mode, description. `work_mode` must be `Remote`, `Hybrid`, or `On-site`.

## Endpoints

| Method | Route | Purpose |
|---|---|---|
| GET | `/api/health` | Check API and database |
| GET | `/api/internships?page=1&limit=10` | List with pagination |
| GET | `/api/internships?domain=Web%20Development&work_mode=Remote` | Filter list |
| GET | `/api/internships/:id` | Read one |
| POST | `/api/internships` | Create |
| PUT | `/api/internships/:id` | Replace editable fields |
| PATCH | `/api/internships/:id` | Update selected fields |
| DELETE | `/api/internships/:id` | Delete |

Pagination defaults to page 1 and limit 10; limit must be 1–100. Filters: `domain`, `work_mode`, `location`, `company`. Combine filters as needed.

## cURL examples

List:
```bash
curl "http://localhost:3000/api/internships?page=1&limit=3"
```

Detail:
```bash
curl "http://localhost:3000/api/internships/1"
```

Create:
```bash
curl -X POST "http://localhost:3000/api/internships" -H "Content-Type: application/json" -d "{"title":"API Developer Intern","company":"Demo Company","domain":"Web Development","location":"Remote, India","work_mode":"Remote","description":"Help build and test API endpoints.","skills":["Node.js","Express"],"application_url":"https://example.com/apply","stipend":"₹10,000/month"}"
```

Partial update:
```bash
curl -X PATCH "http://localhost:3000/api/internships/1" -H "Content-Type: application/json" -d "{"stipend":"₹12,000/month"}"
```

Delete:
```bash
curl -X DELETE "http://localhost:3000/api/internships/1"
```

## Response conventions
Success responses use `{"success":true,"data":...}`. List responses also include `pagination` with page, limit, total and totalPages. Errors use `{"success":false,"error":{"code":"...","message":"...","details":[]}}`.

- `400`: invalid JSON, ID, pagination, filters or body fields
- `404`: record or route not found
- `201`: created
- `204`: deleted (empty response)
- `500`: unexpected server error

Validation checks required text, string lengths, allowed work modes, skills array, HTTP/HTTPS URLs and unknown fields.

## Project structure
```text
internship-rest-api/
├── data/seed-internships.json
├── src/db.js
├── src/internships.js
├── src/seed.js
├── src/server.js
├── src/validators.js
├── test/validators.test.js
├── .env.example
├── .gitignore
├── package.json
└── README.md
```

## Submission / deployment
GitHub Pages cannot run a Node API. Push this repository to GitHub for code review, or deploy to a Node.js host with a persistent writable disk for SQLite. Configure `DB_PATH` if your host requires a different database location. Do not commit secrets or a production database.

Educational starter project; review and test it before submission.

