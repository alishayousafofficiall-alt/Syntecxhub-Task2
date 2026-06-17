# Syntecxhub-Task2
# GridFS File Manager

Full-stack file upload/retrieval/delete system using **Node.js + Express + Multer + MongoDB GridFS**.

---

## Project Structure

```
gridfs-app/
├── backend/
│   ├── config/
│   │   └── db.js              # MongoDB + GridFS connection
│   ├── middleware/
│   │   └── upload.js          # Multer + GridFsStorage + validation
│   ├── routes/
│   │   └── files.js           # All file API routes
│   ├── server.js              # Express entry point
│   ├── .env.example
│   └── package.json
└── frontend/
    └── index.html             # Single-file frontend UI
```

---

## Setup & Run

### 1. Prerequisites
- Node.js v18+
- MongoDB running locally on port 27017 (or provide Atlas URI)

### 2. Backend

```bash
cd backend
npm install
cp .env.example .env        # edit MONGO_URI if needed
npm start                   # or: npm run dev (with nodemon)
```

Server starts at → `http://localhost:5000`

### 3. Frontend

Just open `frontend/index.html` in your browser.
> If you get CORS errors, serve it with VS Code Live Server or:
```bash
npx serve frontend
```

---

## API Reference

| Method | Endpoint | Description |
|--------|----------|-------------|
| `POST` | `/api/files/upload` | Upload single file (field: `file`) |
| `POST` | `/api/files/upload-multiple` | Upload up to 5 files (field: `files`) |
| `GET`  | `/api/files` | List files — supports `?page=&limit=&search=` |
| `GET`  | `/api/files/:id` | Get metadata for one file |
| `GET`  | `/api/files/:id/download` | Stream/download file content |
| `DELETE` | `/api/files/:id` | Delete a file from GridFS |
| `GET`  | `/health` | Server health check |

---

## Validation Rules

- **Allowed types:** JPEG, PNG, GIF, WebP, PDF, DOC/DOCX, TXT, ZIP, MP4, MP3
- **Max file size:** 10 MB per file
- **Max files (multi-upload):** 5

---

## Example Requests (curl)

```bash
# Upload single
curl -F "file=@photo.jpg" http://localhost:5000/api/files/upload

# Upload multiple
curl -F "files=@a.pdf" -F "files=@b.png" http://localhost:5000/api/files/upload-multiple

# List files
curl "http://localhost:5000/api/files?page=1&limit=10"

# Search files
curl "http://localhost:5000/api/files?search=photo"

# Download
curl -OJ http://localhost:5000/api/files/<id>/download

# Delete
curl -X DELETE http://localhost:5000/api/files/<id>
```
