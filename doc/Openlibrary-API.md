# OpenLibrary Subjects API

## Overview
This API provides access to subject-related works from Open Library. It allows filtering by optional query parameters such as `details`, `ebooks`, and `published_in`.

**Base URL:** `https://openlibrary.org`

---

## Endpoint

### `GET /subjects/{subject}.json`
Retrieve a list of works and subject categories linked to a specific subject.

#### Path Parameters

| Name    | Type   | Required | Description                                |
|---------|--------|----------|--------------------------------------------|
| subject | string | Yes      | The main subject keyword (e.g. "love")     |

#### Query Parameters

| Name         | Type    | Required | Default | Description                                                        |
|--------------|---------|----------|---------|--------------------------------------------------------------------|
| details      | boolean | No       | false   | Whether to include detailed information for each work             |
| ebooks       | boolean | No       | false   | Return only works with ebooks available                           |
| published_in | string  | No       | None    | A year or range (e.g. "1500-1600") to filter works by published date |

---

## Responses

### 200 OK
Returns a list of related subjects and associated works.

#### Response Body Schema

**SubjectList**

| Field         | Type    | Description                                        |
|---------------|---------|----------------------------------------------------|
| key           | string  | Subject key path (e.g., `/subjects/love`)          |
| name          | string  | The subject name (e.g., `love`)                    |
| subject_type  | string  | Type of subject (usually `subject`)                |
| work_count    | integer | Total number of works under this subject           |
| works         | array   | Array of work objects (see **Work Object** below)  |

**Work Object**

| Field              | Type     | Description                                |
|-------------------|----------|--------------------------------------------|
| key               | string   | Work key (e.g., `/works/OL362427W`)        |
| title             | string   | Title of the work                          |
| edition_count     | integer  | Number of editions available               |
| cover_id          | integer  | Cover image ID                             |
| cover_edition_key | string   | Key of the edition used for cover          |
| subject           | array    | List of subject strings related to the work|

---

### 404 Not Found
Returned when the subject is not found in the Open Library database.

#### Example
```json
{
  "status": 404,
  "message": "Subject not found in the database."
}
```

---

### 500 Internal Server Error
Returned when there is an unexpected error on the server.

#### Example
```json
{
  "status": 500,
  "message": "An unexpected error occurred. Please try again later."
}
```

---

## Component Schemas

### SubjectList

| Field        | Type    | Example           |
|--------------|---------|-------------------|
| key          | string  | `/subjects/love`  |
| name         | string  | `love`            |
| subject_type | string  | `subject`         |
| work_count   | integer | `74`              |
| works        | array   | Array of works    |

### Work (within works array)

| Field              | Type     | Example              |
|-------------------|----------|----------------------|
| key               | string   | `/works/OL362427W`   |
| title             | string   | `Romeo and Juliet`   |
| edition_count     | integer  | `973`                |
| cover_id          | integer  | `8257991`            |
| cover_edition_key | string   | `OL26501367M`        |
| subject           | array    | `["Drama", "Love"]`  |

### ErrorResponse

| Field   | Type    | Example                              |
|---------|---------|--------------------------------------|
| status  | integer | `404`                                |
| message | string  | `Subject not found in the database.` |
