# SoundScape

A full-stack music streaming web application with separate **Listener** and **Artist** experiences, built from scratch using a custom Node.js HTTP server and MySQL database — no frameworks like Express used.

---

## Features

### Listener
- Browse and stream songs and albums with a built-in audio player
- Play, pause, skip forward/backward through the song library
- Add/remove songs from a **Favorites** list
- Create playlists with custom cover images
- Add and remove songs from playlists via a dropdown menu
- Edit profile — update bio and profile picture

### Artist
- Upload songs with cover art directly from the dashboard
- Upload albums with cover art
- View your own uploaded songs and albums separately from the global library

### General
- User authentication — login and signup with role selection (listener or artist)
- Session persistence via `sessionStorage`
- Dynamic frontend using `XMLHttpRequest` (no fetch API or frontend framework)

---

## Tech Stack

| Layer | Technology |
|---|---|
| Backend | Node.js (custom HTTP server, no Express) |
| Database | MySQL (hosted on Google Cloud SQL) |
| Frontend | HTML, CSS, Vanilla JavaScript |
| File Uploads | Formidable, Multiparty |
| Audio Streaming | HTTP Range Requests (supports seek) |

---

## Project Structure

```
SoundScape/
├── server/
│   └── server.js        # Custom HTTP server, all API routes
├── public_html/
│   ├── login.html
│   ├── signup.html
│   ├── listener_main.html
│   ├── artist_main.html
│   ├── image/           # Profile pics, album art, playlist covers
│   └── music/           # Uploaded MP3 files
├── css/
│   └── *.css
└── js/
    └── main.js          # All frontend logic (auth, player, playlists, uploads)
```

---

## API Routes

| Method | Route | Description |
|---|---|---|
| POST | `/login.html` | Authenticate user, redirect by role |
| POST | `/signup.html` | Register new user |
| GET | `/songs` | Get all songs |
| GET | `/albums` | Get all albums |
| GET | `/artist_songs` | Get songs by artist |
| GET | `/artist_albums` | Get albums by artist |
| GET | `/favorites` | Get user's favorited songs |
| POST | `/favorites/add` | Add song to favorites |
| POST | `/favorites/remove` | Remove song from favorites |
| GET | `/playlist/list` | Get all playlists for a user |
| POST | `/playlist/create` | Create playlist with cover image |
| GET | `/playlist/songs` | Get songs in a playlist |
| POST | `/playlist/add-song` | Add song to playlist |
| POST | `/playlist/remove-song` | Remove song from playlist |
| POST | `/playlist/delete` | Delete entire playlist |
| GET | `/profile/info` | Get user profile data |
| POST | `/profile/update` | Update bio and profile picture |
| POST | `/upload` | Upload song or album (artist only) |

---

## Database Schema

The application uses a MySQL database (`musicDatabase`) with the following tables:

- `users` — id, username, password, role, bio, profile_image, date_joined
- `songs` — id, name, artist, cover, path, artist_id
- `albums` — id, name, artist, cover, date_added, artist_id
- `favorites` — user_id, song_id
- `playlists` — id, user_id, name, image, created_on
- `playlist_songs` — playlist_id, song_id

---

## Getting Started

### Prerequisites
- Node.js v14+
- MySQL database (local or hosted)
- npm

### Installation

```bash
git clone https://github.com/stonedscone/SoundScape.git
cd SoundScape/server
npm install
```

### Configuration

In `server.js`, update the database connection pool with your own credentials:

```javascript
const connection_pool = mysql.createPool({
    host: 'YOUR_DB_HOST',
    user: 'YOUR_DB_USER',
    password: 'YOUR_DB_PASSWORD',
    database: 'musicDatabase',
    connectionLimit: 10
});
```

### Run

```bash
node server.js
```

Then open your browser and navigate to `http://localhost:80`

> **Note:** Running on port 80 may require elevated permissions on some systems. Change the port in `server.js` if needed.

---

## Notes

- Audio streaming supports **HTTP Range Requests**, enabling seekable playback in the browser
- File uploads (songs, album art, profile pictures, playlist covers) are handled server-side and stored in `public_html/image/` and `public_html/music/`
- This project was built as a college coursework assignment — the database credentials in the original repo pointed to a class-provided Google Cloud SQL instance
