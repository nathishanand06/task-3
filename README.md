# Internship Records REST API

A beginner-friendly REST API built with Node.js, Express and SQLite. It supports creating, viewing, updating and deleting internship records, input validation, filtering and pagination.

## Requirements

- Node.js 18 or later
- npm

## Setup

```bash
npm install
```

Copy `.env.example` to `.env` if you want to change the default port or database path.

```bash
npm run seed
npm run dev
```

The API runs at `http://localhost:3000`.

The SQLite database is created automatically at `data/internships.db`.

## Internship data model

| Field | Type | Required | Description |
|---|---|---:|---|
| id | integer | generated | Unique record ID |
| title | string | yes | Internship role |
| company | string | yes | Company name |
| location | string | yes | Work location |
| mode | string | yes | Remote, On-site or Hybrid |
| description | string | yes | Role description |
| stipend | number | no | Stipend amount; defaults to 0 |
| duration | string | yes | Internship duration |
| apply_url | string/null | no | Application URL |
| is_paid | boolean | no | Whether the internship is paid |
| created_at | timestamp | generated | Creation time |
| updated_at | timestamp | generated | Last update time |

## Endpoints

| Method | Endpoint | Purpose |
|---|---|---|
| GET | `/api/internships` | List records |
| GET | `/api/internships/:id` | Get one record |
| POST | `/api/internships` | Create a record |
| PUT | `/api/internships/:id` | Update a record |
| DELETE | `/api/internships/:id` | Delete a record |

### List, search and pagination

`GET /api/internships?page=1&limit=10`

Optional filters:

- `search`: searches title, company and location
- `mode`: Remote, On-site or Hybrid

Example: `/api/internships?page=1&limit=5&search=python&mode=Remote`

### Create request

```json
{
  "title": "Backend Developer Intern",
  "company": "Example Technologies",
  "location": "Remote",
  "mode": "Remote",
  "description": "Build and test backend API endpoints.",
  "stipend": 7000,
  "duration": "8 weeks",
  "apply_url": "https://example.com/apply",
  "is_paid": true
}
```

### Update request

`PUT /api/internships/1`

Send only the fields that need to change.

```json
{
  "stipend": 9000,
  "mode": "Hybrid"
}
```

## Response format

Successful list response:

```json
{
  "success": true,
  "data": [],
  "pagination": {
    "page": 1,
    "limit": 10,
    "total": 0,
    "totalPages": 0
  }
}
```

Successful detail/create/update response:

```json
{
  "success": true,
  "data": {}
}
```

Error response:

```json
{
  "success": false,
  "error": {
    "message": "Validation failed",
    "details": {
      "title": "This field is required"
    }
  }
}
```

## Status codes

- `200`: request successful
- `201`: record created
- `400`: invalid input, query or ID
- `404`: record or route not found
- `500`: unexpected server error

## Test with curl

List:
```bash
curl http://localhost:3000/api/internships
```

Get one:
```bash
curl http://localhost:3000/api/internships/1
```

Create:
```bash
curl -X POST http://localhost:3000/api/internships -H "Content-Type: application/json" -d "{\"title\":\"Backend Intern\",\"company\":\"Example Co\",\"location\":\"Remote\",\"mode\":\"Remote\",\"description\":\"Work on APIs\",\"stipend\":5000,\"duration\":\"2 months\",\"is_paid\":true}"
```

Update:
```bash
curl -X PUT http://localhost:3000/api/internships/1 -H "Content-Type: application/json" -d "{\"stipend\":7500}"
```

Delete:
```bash
curl -X DELETE http://localhost:3000/api/internships/1
```

## Short project explanation

This project provides a REST API to manage internship listings. Express handles HTTP requests and routes. SQLite stores records persistently in a local database file. Validation checks required fields, accepted work modes, stipend values and application URLs before data is saved. The list endpoint supports pagination and optional search and mode filters. Each endpoint returns a consistent JSON response and suitable HTTP status code.
