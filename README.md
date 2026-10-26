# IMovies — MERN cinema booking demo

A runnable educational MERN starter with movie discovery, search, showtimes, JWT authentication, role-based admin access, seat selection, booking history, cancellation and MongoDB persistence. Payment is intentionally a demo placeholder; it does not take money.

## Prerequisites
- Node.js 18+ (recommended; Node 16 may work but is end-of-life)
- npm
- MongoDB Community running locally, or a MongoDB Atlas URI

## Run locally
Open two terminals from the project root.

### 1. Backend
```bash
cd server
npm install
cp .env.example .env
# Edit .env and set a long random JWT_SECRET and your MongoDB URI
npm run seed
npm run dev
```
API: http://localhost:5000/api/health

### 2. Frontend
```bash
cd client
npm install
npm run dev
```
Open http://localhost:5173.

Demo admin: `admin@imovies.local` / `Admin123!` (change this immediately if you expose the app beyond your own machine). Register a normal account to test user bookings.

## Booking flow
1. Open a movie and choose a showtime.
2. Select available seats (up to 10 per booking).
3. Sign in/register if needed and confirm.
4. Review or cancel upcoming bookings under My bookings.
5. Sign in as the demo admin to add movies.

## Add a show (admin API)
Use Postman with `POST http://localhost:5000/api/admin/shows`, header `Authorization: Bearer YOUR_ADMIN_JWT`, and JSON body:
```json
{
  "movie": "MONGODB_MOVIE_OBJECT_ID",
  "theatre": "PVR Treasure Island",
  "screen": "Screen 1",
  "city": "Indore",
  "startsAt": "2027-01-15T18:30:00.000Z",
  "price": 220,
  "seatCount": 40
}
```
Get the movie ID from `GET /api/movies`. Get the admin JWT by signing in at `POST /api/auth/login` with `{"email":"admin@imovies.local","password":"Admin123!"}`.

## Important notes
- Seed data creates four sample movies and upcoming shows in Indore. Poster images need an internet connection.
- Seat booking uses a MongoDB transaction; transactions require a replica set (MongoDB Atlas is already configured as a replica set). For local MongoDB, run it as a single-node replica set to test transaction handling, or adapt booking to use a different concurrency-safe strategy.
- This is a learning model, not production-ready. Before deployment: replace demo credentials, use a strong secret, add input validation, CSRF/CSP/security headers, email verification/password reset, audit logs, payment-provider webhooks, refunds, theatre/show admin screens, and production-grade transaction/retry handling. Never store card details yourself.
