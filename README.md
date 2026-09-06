# Car Rental 🚗

A full-stack peer-to-peer car rental marketplace. Users can browse and book available cars; car owners can list their own vehicles, manage bookings, and track earnings from a dedicated dashboard. Built as a MERN application with JWT authentication and image uploads via ImageKit.

## Features

**For renters**
- Browse all listed cars, with availability, location, price per day, and specs (seating capacity, fuel type, transmission)
- Check real-time availability for specific pickup/return dates before booking
- Book a car for a date range
- View personal booking history and status (pending / confirmed / cancelled)

**For owners**
- Switch any account to an "owner" role
- List a car with photos, pricing, and details (image upload handled via ImageKit)
- Toggle a car's availability on/off, or delete a listing
- View and manage all incoming bookings for owned cars, and confirm or cancel them
- Dashboard with an overview of owned cars and booking activity

**Shared**
- JWT-based authentication (register/login)
- Profile image upload

## Tech Stack

| Layer | Technology |
|---|---|
| Frontend | [React](https://react.dev/) (Vite), [React Router](https://reactrouter.com/), [Tailwind CSS](https://tailwindcss.com/), [Axios](https://axios-http.com/), [Framer Motion](https://motion.dev/) |
| Backend | [Node.js](https://nodejs.org/), [Express](https://expressjs.com/) |
| Database | [MongoDB](https://www.mongodb.com/) with [Mongoose](https://mongoosejs.com/) |
| Authentication | [JSON Web Tokens](https://jwt.io/) + [bcrypt](https://www.npmjs.com/package/bcrypt) |
| Image storage | [ImageKit](https://imagekit.io/) |
| File uploads | [Multer](https://www.npmjs.com/package/multer) |

## How It Works

```
User registers/logs in
        ↓
JWT issued, sent as a Bearer token on subsequent requests
        ↓
Browse cars (public) → check availability for chosen dates → create a booking
        ↓
Owner reviews incoming bookings on their dashboard
        ↓
Owner confirms or cancels each booking
```

Car listings and bookings are two separate MongoDB collections — a `Car` document holds the vehicle's details and availability flag, while a `Booking` references both the car and the renting user, along with a pickup/return date range and status. Owner-only routes (`/api/owner/*`) are protected by JWT middleware; car listing and image upload are additionally routed through Multer and ImageKit before the car record is saved.

## Getting Started

### Prerequisites

- Node.js 18+
- A [MongoDB](https://www.mongodb.com/) database (local or Atlas)
- An [ImageKit](https://imagekit.io/) account (for image uploads)

### Installation

```bash
git clone https://github.com/IshanGaurav/Car-Rental.git
cd Car-Rental
```

Install dependencies for both the server and the client:

```bash
cd server && npm install
cd ../client && npm install
```

### Environment variables

Create a `.env` file inside `server/`:

```env
MONGODB_URI=
JWT_SECRET=
PORT=3000

IMAGEKIT_PUBLIC_KEY=
IMAGEKIT_PRIVATE_KEY=
IMAGEKIT_URL_ENDPOINT=
```

Create a `.env` file inside `client/` (Vite requires the `VITE_` prefix):

```env
VITE_BASE_URL=http://localhost:3000
```

### Run locally

Start the backend:

```bash
cd server
npm run server   # nodemon, for development
# or: npm start  # plain node
```

In a separate terminal, start the frontend:

```bash
cd client
npm run dev
```

Visit the URL Vite prints (typically [http://localhost:5173](http://localhost:5173)).

## Project Structure

```
Car-Rental/
├── server/
│   ├── configs/          # DB connection, ImageKit config
│   ├── controllers/      # userController, ownerController, bookingController
│   ├── middleware/       # JWT auth, Multer upload
│   ├── models/           # User, Car, Booking
│   ├── routes/           # /api/user, /api/owner, /api/bookings
│   └── server.js
└── client/
    └── src/
        ├── components/
        │   └── owner/     # Owner-specific UI (nav, sidebar, etc.)
        ├── context/       # Shared app state (auth, cars, etc.)
        └── pages/
            ├── Home.jsx
            ├── Cars.jsx
            ├── CarDetails.jsx
            ├── MyBookings.jsx
            └── owner/
                ├── Layout.jsx
                ├── Dashboard.jsx
                ├── AddCar.jsx
                ├── ManageCars.jsx
                └── ManageBookings.jsx
```

## API Overview

| Route | Method | Auth required | Description |
|---|---|---|---|
| `/api/user/register` | POST | No | Create an account |
| `/api/user/login` | POST | No | Log in, receive a JWT |
| `/api/user/data` | GET | Yes | Get the current user's profile |
| `/api/user/cars` | GET | No | List all available cars |
| `/api/owner/change-role` | POST | Yes | Upgrade the current account to an owner |
| `/api/owner/add-car` | POST | Yes | List a new car (with image upload) |
| `/api/owner/cars` | GET | Yes | List cars owned by the current user |
| `/api/owner/toggle-car` | POST | Yes | Toggle a car's availability |
| `/api/owner/delete-car` | POST | Yes | Remove a car listing |
| `/api/owner/dashboard` | GET | Yes | Owner dashboard summary data |
| `/api/owner/update-image` | POST | Yes | Update the current user's profile image |
| `/api/bookings/check-availability` | POST | No | Check if a car is free for a given date range |
| `/api/bookings/create` | POST | Yes | Create a new booking |
| `/api/bookings/user` | GET | Yes | Get the current user's bookings |
| `/api/bookings/owner` | GET | Yes | Get bookings for the current owner's cars |
| `/api/bookings/change-status` | POST | Yes | Confirm or cancel a booking |

## Scripts

**Server**

| Command | Description |
|---|---|
| `npm run server` | Start the backend with nodemon (auto-restart on changes) |
| `npm start` | Start the backend with plain Node |

**Client**

| Command | Description |
|---|---|
| `npm run dev` | Start the Vite dev server |
| `npm run build` | Production build |
| `npm run preview` | Preview the production build locally |
| `npm run lint` | Run ESLint |

