# JSONPlaceholder API - Test Case Summary

**Source:** `jsonplaceholder-swagger.json` (OpenAPI 3.0.3)
**Base URL:** `https://jsonplaceholder.typicode.com`
**Generated:** 2026-04-16

> JSONPlaceholder is a free fake REST API for testing and prototyping. POST/PUT/PATCH/DELETE requests simulate success but do not persist data.

---

## Overview

| Resource  | Endpoints | CRUD Coverage | Nested Resources |
|-----------|-----------|---------------|------------------|
| Posts     | 7         | Full          | `/posts/{id}/comments` |
| Comments  | 6         | Full          | - |
| Albums    | 7         | Full          | `/albums/{id}/photos` |
| Photos    | 6         | Full          | - |
| Todos     | 6         | Full          | - |
| Users     | 9         | Full          | `/users/{id}/albums`, `/users/{id}/posts`, `/users/{id}/todos` |
| **Total** | **41**    |               |                  |

---

## 1. Posts

### TC-POST-001: List all posts
- **Endpoint:** `GET /posts`
- **Expected:** 200 OK, returns array of 100 posts
- **Validations:**
  - Response is a JSON array
  - Each item has `userId` (int), `id` (int), `title` (string), `body` (string)
  - Array length is 100
- **Filters:** `?userId={id}` returns only posts by that user

### TC-POST-002: Get a post by ID
- **Endpoint:** `GET /posts/{id}`
- **Expected:** 200 OK, returns single post object
- **Validations:**
  - Response `id` matches path parameter
  - All required fields present: `userId`, `id`, `title`, `body`
- **Edge cases:**
  - `GET /posts/0` - 404 Not Found
  - `GET /posts/101` - 404 Not Found
  - `GET /posts/abc` - 404 Not Found

### TC-POST-003: Create a post
- **Endpoint:** `POST /posts`
- **Body:** `{ "title": "foo", "body": "bar", "userId": 1 }`
- **Expected:** 201 Created, returns created post with `id: 101`
- **Validations:**
  - Response includes all submitted fields
  - Response includes generated `id`
  - Content-Type is `application/json`

### TC-POST-004: Replace a post
- **Endpoint:** `PUT /posts/{id}`
- **Body:** `{ "id": 1, "title": "updated", "body": "updated body", "userId": 1 }`
- **Expected:** 200 OK, returns updated post
- **Validations:**
  - Response reflects all replaced fields
  - `id` remains unchanged

### TC-POST-005: Partially update a post
- **Endpoint:** `PATCH /posts/{id}`
- **Body:** `{ "title": "patched title" }`
- **Expected:** 200 OK, returns post with patched field
- **Validations:**
  - Only the patched field changes
  - Other fields remain intact

### TC-POST-006: Delete a post
- **Endpoint:** `DELETE /posts/{id}`
- **Expected:** 200 OK, returns empty object `{}`
- **Validations:**
  - Response body is `{}`
  - Subsequent GET still returns the post (non-persistent API)

### TC-POST-007: List comments for a post
- **Endpoint:** `GET /posts/{id}/comments`
- **Expected:** 200 OK, returns array of comments
- **Validations:**
  - All returned comments have `postId` matching the path parameter
  - Each comment has `postId`, `id`, `name`, `email`, `body`

---

## 2. Comments

### TC-CMT-001: List all comments
- **Endpoint:** `GET /comments`
- **Expected:** 200 OK, returns array of 500 comments
- **Validations:**
  - Response is a JSON array
  - Each item has `postId`, `id`, `name`, `email`, `body`
  - Array length is 500
- **Filters:** `?postId={id}` returns only comments for that post

### TC-CMT-002: Get a comment by ID
- **Endpoint:** `GET /comments/{id}`
- **Expected:** 200 OK, returns single comment
- **Validations:**
  - `id` matches path parameter
  - `email` field has valid email format
- **Edge cases:**
  - `GET /comments/0` - 404
  - `GET /comments/501` - 404

### TC-CMT-003: Create a comment
- **Endpoint:** `POST /comments`
- **Body:** `{ "postId": 1, "name": "test", "email": "test@example.com", "body": "test body" }`
- **Expected:** 201 Created
- **Validations:**
  - Response includes generated `id`
  - All submitted fields are echoed back

### TC-CMT-004: Replace a comment
- **Endpoint:** `PUT /comments/{id}`
- **Body:** `{ "postId": 1, "id": 1, "name": "replaced", "email": "new@example.com", "body": "new body" }`
- **Expected:** 200 OK
- **Validations:**
  - All fields reflect the replacement

### TC-CMT-005: Partially update a comment
- **Endpoint:** `PATCH /comments/{id}`
- **Body:** `{ "body": "patched body" }`
- **Expected:** 200 OK
- **Validations:**
  - Only patched field changes

### TC-CMT-006: Delete a comment
- **Endpoint:** `DELETE /comments/{id}`
- **Expected:** 200 OK, returns `{}`

---

## 3. Albums

### TC-ALB-001: List all albums
- **Endpoint:** `GET /albums`
- **Expected:** 200 OK, returns array of 100 albums
- **Validations:**
  - Each item has `userId` (int), `id` (int), `title` (string)
  - Array length is 100
- **Filters:** `?userId={id}`

### TC-ALB-002: Get an album by ID
- **Endpoint:** `GET /albums/{id}`
- **Expected:** 200 OK
- **Validations:**
  - `id` matches path parameter
  - Required fields: `userId`, `id`, `title`
- **Edge cases:**
  - `GET /albums/0` - 404
  - `GET /albums/101` - 404

### TC-ALB-003: Create an album
- **Endpoint:** `POST /albums`
- **Body:** `{ "userId": 1, "title": "new album" }`
- **Expected:** 201 Created

### TC-ALB-004: Replace an album
- **Endpoint:** `PUT /albums/{id}`
- **Expected:** 200 OK

### TC-ALB-005: Partially update an album
- **Endpoint:** `PATCH /albums/{id}`
- **Expected:** 200 OK

### TC-ALB-006: Delete an album
- **Endpoint:** `DELETE /albums/{id}`
- **Expected:** 200 OK, returns `{}`

### TC-ALB-007: List photos for an album
- **Endpoint:** `GET /albums/{id}/photos`
- **Expected:** 200 OK, returns array of photos
- **Validations:**
  - All returned photos have `albumId` matching path parameter
  - Each photo has `albumId`, `id`, `title`, `url`, `thumbnailUrl`

---

## 4. Photos

### TC-PHO-001: List all photos
- **Endpoint:** `GET /photos`
- **Expected:** 200 OK, returns array of 5000 photos
- **Validations:**
  - Each item has `albumId`, `id`, `title`, `url`, `thumbnailUrl`
  - Array length is 5000
- **Filters:** `?albumId={id}`

### TC-PHO-002: Get a photo by ID
- **Endpoint:** `GET /photos/{id}`
- **Expected:** 200 OK
- **Validations:**
  - Required fields: `albumId`, `id`, `title`, `url`, `thumbnailUrl`
  - `url` and `thumbnailUrl` are valid URL format
- **Edge cases:**
  - `GET /photos/0` - 404
  - `GET /photos/5001` - 404

### TC-PHO-003: Create a photo
- **Endpoint:** `POST /photos`
- **Body:** `{ "albumId": 1, "title": "test photo", "url": "https://example.com/photo.jpg", "thumbnailUrl": "https://example.com/thumb.jpg" }`
- **Expected:** 201 Created

### TC-PHO-004: Replace a photo
- **Endpoint:** `PUT /photos/{id}`
- **Expected:** 200 OK

### TC-PHO-005: Partially update a photo
- **Endpoint:** `PATCH /photos/{id}`
- **Expected:** 200 OK

### TC-PHO-006: Delete a photo
- **Endpoint:** `DELETE /photos/{id}`
- **Expected:** 200 OK, returns `{}`

---

## 5. Todos

### TC-TODO-001: List all todos
- **Endpoint:** `GET /todos`
- **Expected:** 200 OK, returns array of 200 todos
- **Validations:**
  - Each item has `userId` (int), `id` (int), `title` (string), `completed` (boolean)
  - Array length is 200
- **Filters:** `?userId={id}`, `?completed={true|false}`

### TC-TODO-002: Get a todo by ID
- **Endpoint:** `GET /todos/{id}`
- **Expected:** 200 OK
- **Validations:**
  - `completed` is a boolean, not a string
- **Edge cases:**
  - `GET /todos/0` - 404
  - `GET /todos/201` - 404

### TC-TODO-003: Create a todo
- **Endpoint:** `POST /todos`
- **Body:** `{ "userId": 1, "title": "new task", "completed": false }`
- **Expected:** 201 Created

### TC-TODO-004: Replace a todo
- **Endpoint:** `PUT /todos/{id}`
- **Expected:** 200 OK

### TC-TODO-005: Partially update a todo
- **Endpoint:** `PATCH /todos/{id}`
- **Body:** `{ "completed": true }`
- **Expected:** 200 OK

### TC-TODO-006: Delete a todo
- **Endpoint:** `DELETE /todos/{id}`
- **Expected:** 200 OK, returns `{}`

---

## 6. Users

### TC-USR-001: List all users
- **Endpoint:** `GET /users`
- **Expected:** 200 OK, returns array of 10 users
- **Validations:**
  - Each user has `id`, `name`, `username`, `email`, `address`, `phone`, `website`, `company`
  - `address` contains nested `street`, `suite`, `city`, `zipcode`, `geo` (with `lat`/`lng`)
  - `company` contains `name`, `catchPhrase`, `bs`
  - Array length is 10
- **Filters:** `?email={email}`, `?username={username}`

### TC-USR-002: Get a user by ID
- **Endpoint:** `GET /users/{id}`
- **Expected:** 200 OK
- **Validations:**
  - Nested `address.geo.lat` and `address.geo.lng` are string-encoded coordinates
  - `email` is valid format
- **Edge cases:**
  - `GET /users/0` - 404
  - `GET /users/11` - 404

### TC-USR-003: Create a user
- **Endpoint:** `POST /users`
- **Body:** Full user object with nested address/company
- **Expected:** 201 Created

### TC-USR-004: Replace a user
- **Endpoint:** `PUT /users/{id}`
- **Expected:** 200 OK

### TC-USR-005: Partially update a user
- **Endpoint:** `PATCH /users/{id}`
- **Expected:** 200 OK

### TC-USR-006: Delete a user
- **Endpoint:** `DELETE /users/{id}`
- **Expected:** 200 OK, returns `{}`

### TC-USR-007: List albums for a user
- **Endpoint:** `GET /users/{id}/albums`
- **Expected:** 200 OK
- **Validations:**
  - All returned albums have `userId` matching path parameter

### TC-USR-008: List posts for a user
- **Endpoint:** `GET /users/{id}/posts`
- **Expected:** 200 OK
- **Validations:**
  - All returned posts have `userId` matching path parameter

### TC-USR-009: List todos for a user
- **Endpoint:** `GET /users/{id}/todos`
- **Expected:** 200 OK
- **Validations:**
  - All returned todos have `userId` matching path parameter

---

## 7. Cross-Cutting Test Cases

### TC-CC-001: Content-Type header
- All endpoints return `Content-Type: application/json; charset=utf-8`

### TC-CC-002: CORS headers
- All responses include appropriate CORS headers for cross-origin requests

### TC-CC-003: Invalid resource ID format
- Non-numeric IDs return 404 for all resource types

### TC-CC-004: HTTP method not allowed
- Unsupported methods on valid paths return 404 or 405

### TC-CC-005: Empty body on POST
- `POST` with no body returns created resource with null/default fields

### TC-CC-006: Extra fields in request body
- Extra fields in POST/PUT/PATCH are accepted (no strict validation)

### TC-CC-007: Relationship consistency
- Posts reference valid `userId` values (1-10)
- Comments reference valid `postId` values (1-100)
- Albums reference valid `userId` values (1-10)
- Photos reference valid `albumId` values (1-100)
- Todos reference valid `userId` values (1-10)

### TC-CC-008: Nested resource consistency
- `GET /posts/{id}/comments` returns same results as `GET /comments?postId={id}`
- `GET /albums/{id}/photos` returns same results as `GET /photos?albumId={id}`
- `GET /users/{id}/posts` returns same results as `GET /posts?userId={id}`
- `GET /users/{id}/albums` returns same results as `GET /albums?userId={id}`
- `GET /users/{id}/todos` returns same results as `GET /todos?userId={id}`

---

## Test Case Summary Statistics

| Category            | Count |
|---------------------|-------|
| Posts               | 7     |
| Comments            | 6     |
| Albums              | 7     |
| Photos              | 6     |
| Todos               | 6     |
| Users               | 9     |
| Cross-Cutting       | 8     |
| **Total Test Cases**| **49**|
