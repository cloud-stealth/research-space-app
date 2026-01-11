# Research Space

**Modern Full-Stack File Management Application with S3-Compatible Storage**

[![React](https://img.shields.io/badge/React-19.2.3-61DAFB.svg)](https://reactjs.org/)
[![TypeScript](https://img.shields.io/badge/TypeScript-5.9-blue.svg)](https://www.typescriptlang.org/)
[![Node.js](https://img.shields.io/badge/Node.js-18+-green.svg)](https://nodejs.org/)
[![Express](https://img.shields.io/badge/Express-4.21-lightgrey.svg)](https://expressjs.com/)
[![Prisma](https://img.shields.io/badge/Prisma-6.19-2D3748.svg)](https://www.prisma.io/)
[![License](https://img.shields.io/badge/License-MIT-yellow.svg)](LICENSE)

---

## 📋 Table of Contents

- [Overview](#overview)
- [Key Features](#key-features)
- [Technology Stack](#technology-stack)
- [Quick Start](#quick-start)
  - [Prerequisites](#prerequisites)
  - [Frontend Setup](#frontend-setup)
  - [Backend Setup](#backend-setup)
  - [Running the Application](#running-the-application)
- [Architecture Overview](#architecture-overview)
- [Project Structure](#project-structure)
- [Documentation Gateway](#documentation-gateway)
- [Development Workflow](#development-workflow)
- [Testing](#testing)
- [Deployment](#deployment)
- [Contributing](#contributing)
- [License](#license)

---

## Overview

**Research Space** is a production-ready, full-stack file management application designed for secure cloud storage operations. It provides a modern, Google Drive-like interface with robust backend APIs supporting multiple S3-compatible storage providers including AWS S3, Cloudflare R2, and MinIO.

Built with cutting-edge technologies, Research Space emphasizes:

- 🎯 **Type Safety**: Full TypeScript coverage across frontend and backend
- ⚡ **Performance**: Optimized with code splitting, lazy loading, and efficient state management
- 🔐 **Security**: AES-256-GCM credential encryption, rate limiting, and CORS protection
- 🎨 **Modern UI**: Shadcn/UI components with Tailwind CSS and 8 custom registries
- 📦 **Scalability**: Prisma ORM with support for SQLite (dev) and PostgreSQL (production)

---

## Key Features

### 🗂️ **File Management**

- Drag-and-drop file/folder operations with @dnd-kit
- Multi-select with shift-click range selection
- Nested folder navigation with breadcrumb trails
- Context menu operations (rename, delete, star, share, duplicate)
- File preview with modal dialogs
- Global drop zone for uploads from anywhere
- Bulk operations (move, delete)

### ☁️ **Storage Integration**

- Multi-provider S3 support (AWS S3, Cloudflare R2, MinIO)
- Presigned URL uploads (direct-to-S3, no backend bandwidth)
- Presigned download URLs with time-limited access
- Connection testing with detailed diagnostics
- Configuration locking mechanism
- Encrypted credential storage

### 📊 **User Experience**

- Real-time upload progress tracking
- Library statistics dashboard
- Dark/light theme support with system preference detection
- Responsive design (mobile, tablet, desktop)
- Toast notifications for user feedback
- Keyboard shortcuts for power users

### 🔒 **Security & Performance**

- JWT authentication (planned)
- Role-based access control (planned)
- Rate limiting (100 requests/15min)
- Helmet.js security headers
- Response compression
- Route-level code splitting
- React 19 concurrent features

---

## Technology Stack

### **Frontend** (`/src`)

| Category | Technology | Version | Purpose |
|----------|-----------|---------|---------|
| **Framework** | React | 19.2.3 | UI library with concurrent features |
| **Language** | TypeScript | 5.9.3 | Type-safe development |
| **Build Tool** | Vite | 7.3.1 | Lightning-fast HMR and bundling |
| **Styling** | Tailwind CSS | 4.1.14 | Utility-first CSS framework |
| **UI Components** | Shadcn/UI | Latest | Accessible component library |
| **State Management** | Zustand | 5.0.9 | Lightweight state management |
| **Routing** | React Router | 7.12.0 | Client-side routing with lazy loading |
| **Drag & Drop** | @dnd-kit | 6.3.1+ | Accessible drag-and-drop toolkit |
| **Icons** | Lucide React | 0.562.0 | Beautiful icon library |
| **Notifications** | Sonner | 2.0.7 | Toast notifications |
| **Charts** | Recharts | 3.2.1 | Data visualization |

### **Backend** (`/backend`)

| Category | Technology | Version | Purpose |
|----------|-----------|---------|---------|
| **Runtime** | Node.js | 18+ | JavaScript runtime |
| **Framework** | Express.js | 4.21.2 | Web application framework |
| **Language** | TypeScript | 5.7.2 | Type-safe backend development |
| **ORM** | Prisma | 6.19.0 | Type-safe database client |
| **Database** | SQLite/PostgreSQL | - | Development/Production databases |
| **Storage SDK** | AWS SDK v3 | 3.966.0 | S3-compatible storage operations |
| **Security** | Helmet | 8.1.0 | Security middleware |
| **Validation** | Joi + Zod | 17.13.3 + 3.25.76 | Input validation |
| **Authentication** | JWT + bcrypt | Latest | Token-based auth (planned) |
| **Logging** | Winston | 3.19.0 | Application logging |

---

## Quick Start

### Prerequisites

Before you begin, ensure you have the following installed:

- **Node.js** 18+ ([Download](https://nodejs.org/))
- **npm** 9+ or **yarn** 1.22+ (comes with Node.js)
- **Git** ([Download](https://git-scm.com/))
- **S3-compatible storage** account (optional for full functionality):
  - AWS S3 ([Sign up](https://aws.amazon.com/s3/))
  - Cloudflare R2 ([Sign up](https://www.cloudflare.com/products/r2/))
  - MinIO ([Local setup guide](docs/guide/MINIO_SETUP_GUIDE.md))

### Frontend Setup

1. **Clone the repository**

```bash
git clone https://github.com/PRATS-gits/research-vite-app.git
cd research-vite-app
```

1. **Install frontend dependencies**

```bash
npm install
```

1. **Configure environment** (optional)

Create a `.env` file in the root directory:

```env
VITE_API_BASE_URL=http://localhost:3001
```

1. **Start development server**

```bash
npm run dev
```

The frontend will be available at `http://localhost:5173`

### Backend Setup

1. **Navigate to backend directory**

```bash
cd backend
```

1. **Install backend dependencies**

```bash
npm install
```

1. **Configure environment**

Create a `.env` file in the backend directory:

```bash
# Generate encryption key
node -e "console.log(require('crypto').randomBytes(32).toString('base64').slice(0, 32))"
```

Then create `.env`:

```env
# Database (SQLite for development)
DATABASE_URL="file:./prisma/dev.db"

# Security
ENCRYPTION_KEY="your-32-character-encryption-key"
API_KEY="your-api-key-here"
ADMIN_PASSWORD="your-admin-password"

# Server
PORT=3001
NODE_ENV=development
CORS_ORIGIN="http://localhost:5173"

# S3 Configuration (will be set via UI)
# Credentials are encrypted and stored in database
```

1. **Setup database**

```bash
# Generate Prisma Client
npx prisma generate

# Run migrations
npx prisma migrate dev

# (Optional) Seed database
npm run db:seed
```

1. **Start backend server**

```bash
npm run dev
```

The backend API will be available at `http://localhost:3001`

### Running the Application

**Option 1: Run both servers separately** (recommended for development)

```bash
# Terminal 1 - Frontend
npm run dev

# Terminal 2 - Backend
cd backend
npm run dev
```

**Option 2: Run both servers concurrently** (requires concurrently package)

```bash
# From root directory
npm run dev:all
```

**Access the application:**

- Frontend: `http://localhost:5173`
- Backend API: `http://localhost:3001`
- Backend Health: `http://localhost:3001/health`

---

## Architecture Overview

Research Space follows a **client-server architecture** with clear separation of concerns:

```
┌─────────────────────────────────────────────────────────────┐
│                     React Frontend (Vite)                   │
│                    http://localhost:5173                    │
├─────────────────────────────────────────────────────────────┤
│  Pages: Library, Connections, Agents, Settings, Status     │
│  Components: FileCard, FolderCard, DndGrid, Modals, Forms  │
│  State: Zustand stores (library, connection, upload, theme)│
│  API Client: filesApi.ts, storageApi.ts (HTTP requests)    │
└────────────────────────┬────────────────────────────────────┘
                         │
                         │ HTTP/REST API
                         │ JSON payloads
                         ↓
┌─────────────────────────────────────────────────────────────┐
│                  Express.js Backend API                     │
│                   http://localhost:3001                     │
├─────────────────────────────────────────────────────────────┤
│  Routes: /api/storage, /api/files, /api/folders           │
│  Controllers: Business logic and request handling          │
│  Services: S3 providers, encryption, presigned URLs        │
│  Middleware: Validation, auth, rate limiting, security     │
└────────────┬───────────────────────┬────────────────────────┘
             │                       │
             │ Prisma ORM            │ AWS SDK v3
             ↓                       ↓
┌──────────────────────┐  ┌──────────────────────────────────┐
│  SQLite/PostgreSQL   │  │   S3-Compatible Storage          │
│  Database            │  │   (AWS S3, R2, MinIO)            │
│                      │  │                                  │
│  - StorageConfig     │  │   - File uploads (presigned)     │
│  - Files metadata    │  │   - File downloads (presigned)   │
│  - Folders hierarchy │  │   - Server-side copy (duplicate) │
│  - Config locks      │  │   - Bucket operations            │
└──────────────────────┘  └──────────────────────────────────┘
```

### **Key Architectural Decisions**

1. **Presigned URLs**: Direct client-to-S3 uploads/downloads eliminate backend bandwidth bottlenecks
2. **Prisma ORM**: Type-safe database operations with automated migrations
3. **Provider Abstraction**: Unified interface for AWS S3, Cloudflare R2, and MinIO
4. **Zustand State**: Lightweight state management avoiding Redux complexity
5. **Code Splitting**: Route-level lazy loading for optimal bundle sizes
6. **Soft Delete**: Files/folders marked deleted but recoverable

For detailed architecture documentation, see [`PROJECT_OVERVIEW.md`](PROJECT_OVERVIEW.md)

---

## Project Structure

```
research-vite-app/
├── src/                        # Frontend source code
│   ├── api/                    # HTTP client for backend APIs
│   ├── components/             # React components (library, connections, layout, UI)
│   ├── hooks/                  # Custom React hooks
│   ├── pages/                  # Route pages (Library, Connections, Settings, etc.)
│   ├── router/                 # React Router configuration
│   ├── store/                  # Zustand state stores
│   ├── types/                  # TypeScript type definitions
│   ├── services/               # Frontend business logic
│   └── main.tsx                # Application entry point
│
├── backend/                    # Backend API server
│   ├── src/                    # Backend source code
│   │   ├── controllers/        # Request handlers
│   │   ├── routes/             # API route definitions
│   │   ├── services/           # Business logic (S3 providers, encryption)
│   │   ├── middleware/         # Express middleware (auth, validation)
│   │   ├── models/             # Prisma client and utilities
│   │   ├── types/              # TypeScript interfaces
│   │   ├── config/             # Configuration files
│   │   └── server.ts           # Express app entry point
│   ├── prisma/                 # Prisma schema and migrations
│   │   ├── schema.prisma       # Database schema definition
│   │   └── migrations/         # Database migration history
│   └── docs/                   # Backend-specific documentation
│
├── docs/                       # Project-wide documentation
│   ├── rules/                  # Coding standards and PLAN rules
│   ├── plans/                  # Domain-specific implementation plans
│   ├── guide/                  # Setup and usage guides
│   └── changes/                # Change logs and bug fixes
│
├── .github/                    # GitHub configuration
│   └── instructions/           # AI agent instructions
│
├── public/                     # Static assets
├── components.json             # Shadcn/UI configuration (8 registries)
├── package.json                # Frontend dependencies and scripts
├── vite.config.ts              # Vite build configuration
├── tsconfig.json               # TypeScript configuration
├── tailwind.config.ts          # Tailwind CSS configuration
└── README.md                   # This file
```

For complete file tree with descriptions, see [`PROJECT_OVERVIEW.md`](PROJECT_OVERVIEW.md)

---

## Documentation Gateway

### **Getting Started**

- 📖 [Project Overview](PROJECT_OVERVIEW.md) - Comprehensive architecture and file structure
- 🚀 [Quick Start Guide](docs/tasks/QUICK_START.md) - Fast setup instructions
- 📋 [API Documentation](docs/guide/API_DOCUMENTATION.md) - High-level API overview

### **Backend Documentation**

- 📘 [Backend README](backend/README.md) - Complete backend guide with examples
- 🔌 [API Reference](backend/docs/guide/API_DOCUMENTATION.md) - Detailed endpoint documentation
- 🚀 [Deployment Guide](backend/docs/guide/DEPLOYMENT_GUIDE.md) - Railway and production deployment
- 🔧 [MinIO Setup](docs/guide/MINIO_SETUP_GUIDE.md) - Local S3-compatible storage

### **Development Standards**

- 📏 [Coding Standards](docs/rules/CODING_STANDARDS.md) - TypeScript/TSX conventions
- 📝 [PLAN Rules](docs/rules/PLAN_RULE.md) - Project planning and documentation standards

### **Implementation Plans**

- 🎨 [Frontend Plan](docs/plans/frontend/FRONTEND_PLAN.md) - Frontend implementation roadmap
- ⚙️ [Backend Plan](docs/plans/backend/BACKEND_PLAN.md) - Backend implementation roadmap

### **AI Agent Instructions**

- 🤖 [Planner Agent](/.github/instructions/planner.instructions.md) - Project coordination protocols
- 🎨 [Frontend Agent](/.github/instructions/frontend.instructions.md) - Frontend development guidelines
- ⚙️ [Backend Agent](/.github/instructions/backend.instructions.md) - Backend development guidelines
- 🎭 [UI/UX Agent](/.github/instructions/ui-ux.instructions.md) - Design system guidelines

---

## Development Workflow

### **Development Scripts**

```bash
# Frontend Development
npm run dev              # Start Vite dev server (http://localhost:5173)
npm run build            # Build for production
npm run preview          # Preview production build
npm run lint             # Run ESLint
npm run type-check       # TypeScript type checking

# Backend Development (from /backend)
cd backend
npm run dev              # Start Express server with hot reload (tsx watch)
npm run build            # Compile TypeScript to JavaScript
npm run start            # Run production build
npm run lint             # Run ESLint

# Database Operations (from /backend)
npx prisma generate      # Generate Prisma Client
npx prisma migrate dev   # Run migrations in development
npx prisma studio        # Open Prisma Studio (GUI)
npx prisma db push       # Push schema changes without migrations

# Full-Stack Development (from root)
npm run dev:all          # Run both frontend and backend concurrently
npm run build:all        # Build both frontend and backend
npm run test:all         # Run all tests (frontend + backend)
```

### **Development Best Practices**

1. **Branch Strategy**: Use feature branches (`feature/name`) and PR workflow
2. **Commit Convention**: Follow [Conventional Commits](https://www.conventionalcommits.org/)
   - `feat:` New features
   - `fix:` Bug fixes
   - `docs:` Documentation changes
   - `refactor:` Code refactoring
   - `test:` Adding tests
   - `chore:` Maintenance tasks

3. **Code Quality**:
   - Run `npm run lint` before commits
   - Ensure TypeScript strict mode compliance
   - Write meaningful component and function documentation
   - Follow [Coding Standards](docs/rules/CODING_STANDARDS.md)

4. **Testing**:
   - Write unit tests for business logic
   - Integration tests for API endpoints
   - E2E tests for critical user flows (planned)

---

## Testing

### **Frontend Testing** (Planned)

```bash
# Unit tests with Vitest
npm run test

# Component tests with Testing Library
npm run test:components

# E2E tests with Playwright (planned)
npm run test:e2e
```

### **Backend Testing**

```bash
cd backend

# Unit tests with Jest
npm run test

# Integration tests
npm run test:integration

# Test coverage
npm run test:coverage
```

---

## Deployment

### **Frontend Deployment**

**Vercel (Recommended)**

```bash
# Install Vercel CLI
npm i -g vercel

# Deploy
vercel
```

**Netlify**

```bash
# Build command
npm run build

# Publish directory
dist
```

### **Backend Deployment**

**Railway (Recommended)**

See [Backend Deployment Guide](backend/docs/guide/DEPLOYMENT_GUIDE.md) for complete Railway setup instructions.

**Manual VPS Deployment**

```bash
# Build backend
cd backend
npm run build

# Run with PM2
pm2 start dist/server.js --name research-backend
```

### **Environment Variables**

Ensure all required environment variables are set in your deployment platform:

**Frontend:**

- `VITE_API_BASE_URL` - Backend API URL

**Backend:**

- `DATABASE_URL` - PostgreSQL connection string (production)
- `ENCRYPTION_KEY` - 32-character encryption key
- `API_KEY` - API authentication key
- `ADMIN_PASSWORD` - Admin password for sensitive operations
- `CORS_ORIGIN` - Frontend URL for CORS
- `NODE_ENV` - `production`

---

## Contributing

We welcome contributions! Please see our [Contributing Guidelines](CONTRIBUTING.md) for details on:

- Getting started with development
- Branch naming conventions
- Commit message format
- Pull request process
- Code style guidelines

---

## License

This project is licensed under the **MIT License** - see the [LICENSE](LICENSE) file for details.

---

## Acknowledgments

- **Shadcn/UI** - Beautiful component library
- **Radix UI** - Accessible primitives
- **Tailwind CSS** - Utility-first CSS framework
- **Prisma** - Next-generation ORM
- **AWS SDK** - S3-compatible storage operations

---

## Support

For issues, questions, or contributions:

- 📧 Email: [support@researchspace.dev](mailto:support@researchspace.dev)
- 🐛 Issues: [GitHub Issues](https://github.com/PRATS-gits/research-vite-app/issues)
- 💬 Discussions: [GitHub Discussions](https://github.com/PRATS-gits/research-vite-app/discussions)

---

**Built with ❤️ by the Research Space Team**
