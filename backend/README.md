# Research Space Backend API

**Modern S3-Compatible Storage Management Backend**

[![Node.js](https://img.shields.io/badge/Node.js-18+-green.svg)](https://nodejs.org/)
[![TypeScript](https://img.shields.io/badge/TypeScript-5.7-blue.svg)](https://www.typescriptlang.org/)
[![Express](https://img.shields.io/badge/Express-4.21-lightgrey.svg)](https://expressjs.com/)
[![Prisma](https://img.shields.io/badge/Prisma-6.19-2D3748.svg)](https://www.prisma.io/)
[![License](https://img.shields.io/badge/License-MIT-yellow.svg)](LICENSE)

---

## 📋 Table of Contents

- [Overview](#overview)
- [Features](#features)
- [Architecture](#architecture)
- [Quick Start](#quick-start)
- [Project Structure](#project-structure)
- [Environment Configuration](#environment-configuration)
- [Database Setup](#database-setup)
- [API Endpoints](#api-endpoints)
- [Development](#development)
- [Deployment](#deployment)
- [Documentation](#documentation)
- [Contributing](#contributing)

---

## Overview

Research Space Backend is a production-ready Express.js API server that provides secure file storage management with support for multiple S3-compatible providers (AWS S3, Cloudflare R2, MinIO). Built with TypeScript, Prisma ORM, and modern security practices.

### Key Highlights

- 🗄️ **Prisma ORM**: Type-safe database operations with PostgreSQL/SQLite
- 🔐 **Secure Storage**: AES-256-GCM encryption for credentials
- 📁 **Folder Hierarchy**: Unlimited nesting with breadcrumb navigation
- 🚀 **Presigned URLs**: Direct S3 uploads without backend bandwidth
- 🎯 **Soft Delete**: Data recovery with soft delete mechanism
- ⚡ **High Performance**: 6-10x faster than JSON storage
- 🔄 **Bulk Operations**: Efficient multi-file operations
- 📊 **Context Menu**: Share links, duplicate files, star favorites

---

## Features

### Storage Management

- ✅ Multi-provider support (AWS S3, Cloudflare R2, MinIO)
- ✅ Encrypted credential storage (AES-256-GCM)
- ✅ Connection testing and validation
- ✅ Configuration locking mechanism

### File Operations

- ✅ Presigned upload URLs (direct S3 upload)
- ✅ Presigned download URLs (time-limited access)
- ✅ File metadata management (CRUD)
- ✅ Bulk delete and move operations
- ✅ File duplication (S3 server-side copy)
- ✅ Star/favorite functionality
- ✅ Shareable public links
- ✅ Pagination and filtering (50 items/page)
- ✅ Soft delete with recovery option

### Folder Management

- ✅ Unlimited folder nesting
- ✅ Folder hierarchy with breadcrumb
- ✅ Recursive folder operations
- ✅ Folder contents listing
- ✅ Rename and reorganization
- ✅ Soft delete (cascading)

### Security & Performance

- ✅ Helmet.js security headers
- ✅ Rate limiting (100 req/15min)
- ✅ CORS protection
- ✅ Request compression
- ✅ TypeScript strict mode
- ✅ Input validation (Joi)
- ✅ Error handling middleware

---

## Architecture

### Technology Stack

```
┌─────────────────────────────────────────────────────┐
│                  Express.js 4.21                    │
│                  (TypeScript 5.7)                   │
├─────────────────────────────────────────────────────┤
│                                                     │
│  ┌──────────────┐  ┌──────────────┐  ┌──────────┐ │
│  │  Controllers │  │   Services   │  │  Models  │ │
│  │              │  │              │  │          │ │
│  │  - Storage   │  │  - Presigned │  │  Prisma  │ │
│  │  - Files     │  │    URL       │  │   ORM    │ │
│  │  - Folders   │  │  - Encryption│  │          │ │
│  │              │  │  - S3/R2     │  │          │ │
│  └──────┬───────┘  └──────┬───────┘  └────┬─────┘ │
│         │                 │               │       │
│         └─────────────────┴───────────────┘       │
│                          │                        │
├──────────────────────────┼────────────────────────┤
│                          │                        │
│              ┌───────────▼──────────┐             │
│              │  PostgreSQL / SQLite │             │
│              │   (Prisma Client)    │             │
│              └──────────────────────┘             │
│                                                     │
│              ┌──────────────────────┐             │
│              │  S3-Compatible       │             │
│              │  Storage Providers   │             │
│              │  - AWS S3            │             │
│              │  - Cloudflare R2     │             │
│              │  - MinIO             │             │
│              └──────────────────────┘             │
└─────────────────────────────────────────────────────┘
```

### Data Flow

**File Upload Flow:**

```
1. Frontend → POST /api/files/presigned-url
2. Backend generates presigned URL (S3)
3. Backend saves metadata → Prisma → PostgreSQL
4. Frontend uploads directly to S3 (no backend bandwidth)
5. Upload complete → File accessible via download URL
```

**File Download Flow:**

```
1. Frontend → POST /api/files/:id/download-url
2. Backend validates file exists (Prisma)
3. Backend generates time-limited presigned URL (S3)
4. Frontend downloads directly from S3
```

---

## Quick Start

### Prerequisites

- **Node.js** 18+ ([Download](https://nodejs.org/))
- **PostgreSQL** 15+ (production) or **SQLite** (development)
- **Git** ([Download](https://git-scm.com/))
- **S3-compatible storage** account (AWS S3, Cloudflare R2, or MinIO)

### Installation

**1. Clone Repository**

```bash
git clone https://github.com/PRATS-gits/research-vite-app.git
cd research-vite-app/backend
```

**2. Install Dependencies**

```bash
npm install
```

**3. Environment Setup**

```bash
cp .env.example .env
```

Edit `.env` with your configuration:

```bash
# Generate secure keys
node -e "console.log(require('crypto').randomBytes(32).toString('base64').slice(0, 32))"

# Example .env
DATABASE_URL="file:./data/database.db"  # SQLite for development
ENCRYPTION_KEY="your-32-char-encryption-key"
API_KEY="your-api-key-here"
ADMIN_PASSWORD="your-admin-password"
PORT=3001
NODE_ENV=development
CORS_ORIGIN="http://localhost:5173"
```

**4. Database Setup**

```bash
# Generate Prisma Client
npx prisma generate

# Run migrations
npx prisma migrate deploy

# (Optional) Seed database
npm run db:seed
```

**5. Start Development Server**

```bash
npm run dev
```

Server runs at: `http://localhost:3001`

**6. Verify Installation**

```bash
curl http://localhost:3001/health
```

Expected response:

```json
{
  "success": true,
  "message": "Server is healthy",
  "data": {
    "status": "ok",
    "timestamp": "2025-10-05T12:00:00.000Z",
    "uptime": 10.5
  }
}
```

---

## Project Structure

```
backend/
├── prisma/                      # Database schema and migrations
│   ├── schema.prisma           # Prisma schema (PostgreSQL/SQLite)
│   ├── migrations/             # Database migrations
│   │   ├── 20251004161543_init/
│   │   └── 20251004162826_add_folder_soft_delete/
│   └── dev.db                  # SQLite database (development)
│
├── src/                         # TypeScript source code
│   ├── server.ts               # Express app entry point
│   │
│   ├── config/                 # Configuration files
│   │   └── [configuration modules]
│   │
│   ├── controllers/            # Request handlers (MVC)
│   │   ├── storage.controller.ts    # Storage config & testing
│   │   ├── files.controller.ts      # File operations & presigned URLs
│   │   └── folders.controller.ts    # Folder CRUD & hierarchy
│   │
│   ├── routes/                 # API route definitions
│   │   ├── storage.routes.ts   # POST /configure, /test | GET /status
│   │   ├── files.routes.ts     # File endpoints (13 routes)
│   │   └── folders.routes.ts   # Folder endpoints (6 routes)
│   │
│   ├── services/               # Business logic layer
│   │   ├── encryption.service.ts       # AES-256-GCM encryption
│   │   ├── presignedUrl.service.ts     # Presigned URL generation
│   │   ├── storageProvider.service.ts  # Provider factory pattern
│   │   ├── s3Provider.service.ts       # AWS S3 implementation
│   │   ├── r2Provider.service.ts       # Cloudflare R2 implementation
│   │   └── minioProvider.service.ts    # MinIO implementation
│   │
│   ├── models/                 # Prisma data models
│   │   ├── storageConfig.model.ts   # Storage configuration
│   │   ├── fileMetadata.model.ts    # File metadata
│   │   ├── folder.model.ts          # Folder hierarchy
│   │   └── uploadQueue.model.ts     # Upload queue management
│   │
│   ├── middleware/             # Express middleware
│   │   ├── auth.middleware.ts       # Admin authentication
│   │   └── validation.middleware.ts # Request validation (Joi)
│   │
│   ├── types/                  # TypeScript type definitions
│   │   ├── storage.types.ts    # Storage-related types
│   │   └── files.types.ts      # File-related types
│   │
│   ├── utils/                  # Utility functions
│   │   └── routeDiscovery.ts   # Auto-discover and display routes
│   │
│   └── scripts/                # Utility scripts
│       ├── migrate-json-to-db.ts    # JSON → Prisma migration
│       └── verify-migration.ts      # Migration verification
│
├── docs/                        # Documentation
│   ├── guide/                  # Guides and tutorials
│   │   └── DEPLOYMENT_GUIDE.md  # Railway deployment guide
│   ├── example/                # API examples and snippets
│   └── misc/                   # Miscellaneous documentation
│
├── data/                        # Runtime data (gitignored)
│   ├── database.db             # SQLite database
│   ├── files.json              # Legacy file metadata (deprecated)
│   ├── folders.json            # Legacy folder data (deprecated)
│   └── storage-config.json     # Legacy storage config (deprecated)
│
├── dist/                        # Compiled JavaScript (gitignored)
│   └── [compiled TypeScript output]
│
├── .env                         # Environment variables (gitignored)
├── .env.example                # Environment template
├── .gitignore                  # Git ignore rules
├── package.json                # Dependencies and scripts
├── tsconfig.json               # TypeScript configuration
└── README.md                   # This file
```

### Key Directories

#### `/prisma`

- **Purpose**: Database schema, migrations, and Prisma Client
- **Migration Files**: Immutable SQL migration history
- **Schema File**: Single source of truth for database structure

#### `/src/controllers`

- **Purpose**: HTTP request handling (Express route handlers)
- **Responsibility**: Parse request → Call services → Format response
- **Examples**:
  - `files.controller.ts`: 13 endpoints for file operations
  - `folders.controller.ts`: 6 endpoints for folder management
  - `storage.controller.ts`: 4 endpoints for storage configuration

#### `/src/services`

- **Purpose**: Business logic implementation
- **Responsibility**: Database operations, external API calls, complex logic
- **Examples**:
  - `presignedUrl.service.ts`: Generates time-limited S3 URLs
  - `encryption.service.ts`: Encrypts/decrypts storage credentials
  - `s3Provider.service.ts`: AWS SDK S3 operations

#### `/src/models`

- **Purpose**: Prisma ORM wrapper models
- **Responsibility**: Database queries, data validation, business rules
- **Pattern**: Each model corresponds to a Prisma schema entity

#### `/src/middleware`

- **Purpose**: Express middleware functions
- **Examples**:
  - `auth.middleware.ts`: Admin password verification
  - `validation.middleware.ts`: Joi schema validation

---

## Environment Configuration

### Environment Variables

| Variable | Required | Default | Description |
|----------|----------|---------|-------------|
| `DATABASE_URL` | ✅ | `file:./data/database.db` | PostgreSQL/SQLite connection string |
| `ENCRYPTION_KEY` | ✅ | - | 32-char AES-256 encryption key |
| `API_KEY` | ✅ | - | API authentication key |
| `ADMIN_PASSWORD` | ✅ | - | Admin override password |
| `PORT` | ⚠️ | `3001` | Server port |
| `NODE_ENV` | ⚠️ | `development` | Environment mode |
| `CORS_ORIGIN` | ⚠️ | `http://localhost:5173` | Allowed frontend origin |
| `LOG_LEVEL` | ❌ | `info` | Winston logging level |

**Legend:**

- ✅ Required for all environments
- ⚠️ Required for production
- ❌ Optional

### Generating Secure Keys

**Using Node.js Crypto:**

```bash
# ENCRYPTION_KEY (32 characters)
node -e "console.log(require('crypto').randomBytes(32).toString('base64').slice(0, 32))"

# API_KEY (48 characters)
node -e "console.log(require('crypto').randomBytes(48).toString('base64'))"

# ADMIN_PASSWORD (32 characters)
node -e "console.log(require('crypto').randomBytes(32).toString('hex'))"
```

**Using OpenSSL:**

```bash
# ENCRYPTION_KEY
openssl rand -base64 32 | cut -c1-32

# API_KEY
openssl rand -base64 48

# ADMIN_PASSWORD
openssl rand -hex 32
```

### Database URL Formats

**SQLite (Development):**

```bash
DATABASE_URL="file:./data/database.db"
```

**PostgreSQL (Production):**

```bash
DATABASE_URL="postgresql://user:password@localhost:5432/research_space?schema=public"
```

**Railway PostgreSQL:**

```bash
DATABASE_URL="${{Postgres.DATABASE_URL}}"
```

---

## Database Setup

### Prisma Schema

Database models defined in `prisma/schema.prisma`:

```prisma
// Storage Configuration (encrypted)
model StorageConfig {
  id            String   @id @default(uuid())
  provider      String   // "aws-s3", "cloudflare-r2", "minio"
  encryptedData String   // AES-256-GCM encrypted JSON
  iv            String   // Initialization vector
  authTag       String   // Authentication tag
  isLocked      Boolean  @default(false)
  createdAt     DateTime @default(now())
  updatedAt     DateTime @updatedAt
}

// File Metadata
model File {
  id        String    @id @default(uuid())
  name      String
  size      Int
  type      String    // MIME type
  s3Key     String    @unique
  folderId  String?
  starred   Boolean   @default(false)
  createdAt DateTime  @default(now())
  updatedAt DateTime  @updatedAt
  deletedAt DateTime? // Soft delete
  
  folder    Folder?   @relation(fields: [folderId])
}

// Folder Hierarchy
model Folder {
  id        String    @id @default(uuid())
  name      String
  parentId  String?
  path      String    // Full path (e.g., "/Documents/Reports")
  starred   Boolean   @default(false)
  createdAt DateTime  @default(now())
  updatedAt DateTime  @updatedAt
  deletedAt DateTime? // Soft delete
  
  parent    Folder?  @relation("FolderHierarchy", fields: [parentId])
  children  Folder[] @relation("FolderHierarchy")
  files     File[]
}
```

### Migration Commands

**Generate Migration:**

```bash
npx prisma migrate dev --name migration_name
```

**Apply Migrations (Production):**

```bash
npx prisma migrate deploy
```

**Reset Database (Development Only):**

```bash
npx prisma migrate reset
```

**View Migration Status:**

```bash
npx prisma migrate status
```

**Generate Prisma Client:**

```bash
npx prisma generate
```

### Database Administration

**Open Prisma Studio:**

```bash
npx prisma studio
```

This opens a web GUI at `http://localhost:5555` for browsing and editing database records.

### Migration History

| Date | Version | Description |
|------|---------|-------------|
| 2025-10-04 | `20251004161543_init` | Initial schema (StorageConfig, File, Folder, ConfigLock) |
| 2025-10-04 | `20251004162826_add_folder_soft_delete` | Added soft delete to folders |

---

## API Endpoints

### Base URL

```
http://localhost:3001
```

### Endpoint Summary

| Category | Method | Endpoint | Description |
|----------|--------|----------|-------------|
| **Health** | GET | `/health` | Server health check |
| **Storage** | GET | `/api/storage/status` | Get storage configuration status |
| **Storage** | POST | `/api/storage/configure` | Configure storage provider |
| **Storage** | POST | `/api/storage/test` | Test storage connection |
| **Storage** | DELETE | `/api/storage/lock` | Remove configuration lock (admin) |
| **Files** | POST | `/api/files/presigned-url` | Generate upload URL |
| **Files** | POST | `/api/files/:id/download-url` | Generate download URL |
| **Files** | POST | `/api/files/:id/preview-url` | Generate preview URL (inline) |
| **Files** | GET | `/api/files/list` | List files (paginated) |
| **Files** | GET | `/api/files/stats` | Get library statistics |
| **Files** | GET | `/api/files/:id` | Get file metadata |
| **Files** | PUT | `/api/files/:id` | Update file metadata |
| **Files** | DELETE | `/api/files/:id` | Delete file (soft delete) |
| **Files** | POST | `/api/files/bulk-delete` | Bulk delete files |
| **Files** | POST | `/api/files/bulk-move` | Bulk move files |
| **Files** | POST | `/api/files/:id/share` | Generate share link |
| **Files** | POST | `/api/files/:id/duplicate` | Duplicate file (S3 copy) |
| **Files** | PUT | `/api/files/:id/star` | Toggle star/favorite |
| **Folders** | POST | `/api/folders` | Create folder |
| **Folders** | GET | `/api/folders/:id` | Get folder details |
| **Folders** | PUT | `/api/folders/:id` | Rename folder |
| **Folders** | DELETE | `/api/folders/:id` | Delete folder (recursive) |
| **Folders** | GET | `/api/folders/:id/contents` | Get folder contents |
| **Folders** | GET | `/api/folders/:id/breadcrumb` | Get breadcrumb path |

**Total: 22 Endpoints**

### Example Requests

**Health Check:**

```bash
curl http://localhost:3001/health
```

**Configure Storage:**

```bash
curl -X POST http://localhost:3001/api/storage/configure \
  -H "Content-Type: application/json" \
  -d '{
    "provider": "aws-s3",
    "credentials": {
      "accessKeyId": "AKIAIOSFODNN7EXAMPLE",
      "secretAccessKey": "wJalrXUtnFEMI/K7MDENG/bPxRfiCYEXAMPLEKEY",
      "region": "us-east-1",
      "bucket": "my-bucket"
    }
  }'
```

**List Files:**

```bash
curl "http://localhost:3001/api/files/list?page=1&limit=50&folderId=null"
```

**Generate Upload URL:**

```bash
curl -X POST http://localhost:3001/api/files/presigned-url \
  -H "Content-Type: application/json" \
  -d '{
    "fileName": "document.pdf",
    "fileType": "application/pdf",
    "fileSize": 1024000,
    "folderId": null
  }'
```

### API Response Format

All endpoints return standardized JSON responses:

**Success Response:**

```json
{
  "success": true,
  "message": "Operation successful",
  "data": { ... },
  "timestamp": "2025-10-05T12:00:00.000Z"
}
```

**Error Response:**

```json
{
  "success": false,
  "error": "Error type",
  "message": "Human-readable error message",
  "timestamp": "2025-10-05T12:00:00.000Z"
}
```

### Detailed API Documentation

For complete API documentation with request/response examples, see:

- **[API Reference](docs/misc/API_REFERENCE.md)** - Complete endpoint documentation
- **[Example Requests](docs/example/)** - Curl examples and Postman collection

---

## Development

### Available Scripts

```json
{
  "dev": "tsx watch src/server.ts",           // Start dev server with hot reload
  "build": "tsc",                             // Compile TypeScript → dist/
  "start": "node dist/server.js",             // Start production server
  "lint": "eslint src --ext .ts",             // Run ESLint
  "test": "jest",                             // Run tests (when implemented)
  "db:generate": "npx prisma generate",       // Generate Prisma Client
  "db:migrate": "npx prisma migrate dev",     // Create & apply migration
  "db:deploy": "npx prisma migrate deploy",   // Apply migrations (prod)
  "db:studio": "npx prisma studio",           // Open Prisma Studio GUI
  "db:seed": "tsx prisma/seed.ts"             // Seed database (if implemented)
}
```

### Development Workflow

**1. Start Development Server:**

```bash
npm run dev
```

Server starts at `http://localhost:3001` with hot reload enabled.

**2. Make Code Changes:**

- Edit files in `src/`
- Server auto-restarts on file changes
- Check terminal for TypeScript errors

**3. Test Changes:**

```bash
# Health check
curl http://localhost:3001/health

# Test specific endpoint
curl http://localhost:3001/api/storage/status
```

**4. Database Changes:**

If you modify `prisma/schema.prisma`:

```bash
# Create migration
npx prisma migrate dev --name descriptive_name

# Regenerate Prisma Client
npx prisma generate
```

**5. View Database:**

```bash
npx prisma studio
```

Opens GUI at `http://localhost:5555`

### Code Style

**TypeScript Configuration:**

- **Strict Mode**: Enabled
- **Target**: ES2022
- **Module**: ESNext
- **Path Aliases**: `@/*` → `src/*`

**Linting:**

```bash
npm run lint
```

### Adding New Features

**1. Create Route:**

```typescript
// src/routes/feature.routes.ts
import { Router } from 'express';
import { FeatureController } from '../controllers/feature.controller.js';

const router = Router();
router.get('/', FeatureController.getAll);
router.post('/', FeatureController.create);

export default router;
```

**2. Create Controller:**

```typescript
// src/controllers/feature.controller.ts
import { Request, Response } from 'express';
import { FeatureModel } from '../models/feature.model.js';

export class FeatureController {
  static async getAll(req: Request, res: Response) {
    const items = await FeatureModel.findAll();
    res.json({ success: true, data: items });
  }
}
```

**3. Create Service (if needed):**

```typescript
// src/services/feature.service.ts
export class FeatureService {
  async processFeature(data: unknown) {
    // Business logic here
    return result;
  }
}
```

**4. Register Route:**

```typescript
// src/server.ts
import featureRoutes from './routes/feature.routes.js';

app.use('/api/features', featureRoutes);
```

---

## Deployment

### Production Build

**1. Compile TypeScript:**

```bash
npm run build
```

**2. Verify Build:**

```bash
ls -la dist/
```

**3. Test Production Build:**

```bash
NODE_ENV=production npm start
```

### Railway Deployment

**Quick Deploy:**

[![Deploy on Railway](https://railway.com/button.svg)](https://railway.com/new)

**Manual Deployment:**

See comprehensive guide: **[Railway Deployment Guide](docs/guide/DEPLOYMENT_GUIDE.md)**

**Key Steps:**

1. Connect GitHub repository
2. Add PostgreSQL database
3. Set environment variables
4. Configure build commands
5. Deploy

**Environment Variables for Railway:**

```bash
DATABASE_URL=${{Postgres.DATABASE_URL}}
ENCRYPTION_KEY=${{secret(32, "chars")}}
API_KEY=${{secret(48, "chars")}}
ADMIN_PASSWORD=${{secret(32, "chars")}}
NODE_ENV=production
PORT=3001
CORS_ORIGIN=https://your-frontend.vercel.app
```

### Docker Deployment

**Dockerfile:**

```dockerfile
FROM node:18-alpine

WORKDIR /app

COPY package*.json ./
RUN npm ci --only=production

COPY prisma ./prisma
RUN npx prisma generate

COPY dist ./dist

EXPOSE 3001

CMD ["npm", "start"]
```

**Build & Run:**

```bash
docker build -t research-space-backend .
docker run -p 3001:3001 --env-file .env research-space-backend
```

---

## Documentation

### Available Documentation

| Document | Description | Location |
|----------|-------------|----------|
| **README.md** | This file - Project overview & quick start | `/backend/README.md` |
| **Deployment Guide** | Railway deployment step-by-step | [`/backend/docs/guide/DEPLOYMENT_GUIDE.md`](docs/guide/DEPLOYMENT_GUIDE.md) |
| **API Reference** | Complete endpoint documentation | `/backend/docs/misc/API_REFERENCE.md` |
| **Development Guide** | Contributing & development setup | `/backend/docs/misc/DEVELOPMENT.md` |
| **Security Guide** | Security best practices | `/backend/docs/misc/SECURITY.md` |
| **Prisma Schema** | Database schema documentation | `/backend/prisma/schema.prisma` |

### External Resources

- **Express.js**: [expressjs.com/en/guide](https://expressjs.com/en/guide/routing.html)
- **Prisma ORM**: [prisma.io/docs](https://www.prisma.io/docs/)
- **TypeScript**: [typescriptlang.org/docs](https://www.typescriptlang.org/docs/)
- **AWS S3**: [docs.aws.amazon.com/s3](https://docs.aws.amazon.com/s3/)
- **Railway**: [docs.railway.com](https://docs.railway.com/)

---

## Contributing

We welcome contributions! Please see the [Contributing Guidelines](../CONTRIBUTING.md) in the project root for:

- Development setup instructions
- Branch naming conventions
- Commit message format (Conventional Commits)
- Pull request guidelines
- Code style requirements

### Reporting Issues

Report bugs or request features via [GitHub Issues](https://github.com/PRATS-gits/research-vite-app/issues)

Include:

- Clear description
- Steps to reproduce (for bugs)
- Expected vs actual behavior
- Environment details (Node version, OS, etc.)

---

## License

This project is licensed under the **MIT License**.

See [LICENSE](LICENSE) file for details.

---

## Support

### Project Maintainers

- **Team**: Research Space Team
- **Repository**: [PRATS-gits/research-vite-app](https://github.com/PRATS-gits/research-vite-app)

### Getting Help

- 📖 **Documentation**: Read guides in `/backend/docs/`
- 🐛 **Issues**: [GitHub Issues](https://github.com/PRATS-gits/research-vite-app/issues)
- 💬 **Discussions**: [GitHub Discussions](https://github.com/PRATS-gits/research-vite-app/discussions)

---

## Changelog

### Version 2.1.0 (2026-01-11)

**📦 Dependency Updates:**

- AWS SDK v3: 3.901.0 → 3.966.0
- Prisma: 6.16.3 → 6.19.0
- Helmet: 8.0.0 → 8.1.0
- Winston: 3.17.0 → 3.19.0

**📚 Documentation:**

- Consolidated contribution guidelines to root CONTRIBUTING.md
- Updated all version references in documentation

---

### Version 1.0.0 (2025-10-05)

**✨ Features:**

- Prisma ORM integration (PostgreSQL/SQLite)
- Multi-provider S3 storage support (AWS, R2, MinIO)
- Presigned URL generation for direct S3 uploads
- Folder hierarchy with unlimited nesting
- Soft delete mechanism with recovery
- Context menu operations (share, duplicate, star)
- Bulk file operations (delete, move)
- Library statistics endpoint
- Secure credential encryption (AES-256-GCM)
- Rate limiting and security headers
- Route auto-discovery utility

**🔧 Infrastructure:**

- TypeScript strict mode
- Express.js 4.21
- Prisma ORM 6.16
- AWS SDK v3
- Helmet.js security
- Joi validation
- Winston logging

**📚 Documentation:**

- Comprehensive README
- Railway deployment guide
- API reference documentation
- Development guidelines

---

**Built with ❤️ by Research Space Team**

**Last Updated**: January 11, 2026
