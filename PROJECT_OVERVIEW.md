# Research Space - Project Overview

**Comprehensive Architecture, Structure, and Technical Documentation Index**

> **Last Updated:** January 11, 2026  
> **Version:** 2.1  
> **Status:** 🟢 Active Development  
> **Type:** Full-Stack TypeScript Application

---

## 📑 Table of Contents

1. [Project Summary](#project-summary)
2. [Technology Stack](#technology-stack)
3. [Architecture Overview](#architecture-overview)
   - [System Architecture](#system-architecture)
   - [Data Flow](#data-flow)
   - [Security Architecture](#security-architecture)
4. [Complete File Structure](#complete-file-structure)
   - [Root Directory](#root-directory)
   - [Frontend Structure (`/src`)](#frontend-structure-src)
   - [Backend Structure (`/backend`)](#backend-structure-backend)
   - [Documentation Structure (`/docs`)](#documentation-structure-docs)
5. [Frontend Architecture Deep Dive](#frontend-architecture-deep-dive)
6. [Backend Architecture Deep Dive](#backend-architecture-deep-dive)
7. [Database Schema](#database-schema)
8. [API Architecture](#api-architecture)
9. [State Management](#state-management)
10. [Component Hierarchy](#component-hierarchy)
11. [Development Status](#development-status)
12. [Documentation Index](#documentation-index)
13. [Setup Instructions](#setup-instructions)
14. [Development Workflow](#development-workflow)

---

## Project Summary

**Research Space** is a modern, production-ready file management application that provides secure cloud storage operations with a Google Drive-like interface. The application features a React-based frontend with TypeScript and a robust Express.js backend API, supporting multiple S3-compatible storage providers.

### **Key Capabilities**

- **Multi-Provider S3 Support**: AWS S3, Cloudflare R2, MinIO
- **Direct Uploads**: Presigned URLs enable client-to-S3 transfers without backend bandwidth
- **Advanced File Operations**: Drag-and-drop, multi-select, nested folders, context menus
- **Secure Credential Storage**: AES-256-GCM encryption for S3 credentials
- **Type-Safe Development**: Full TypeScript coverage across frontend and backend
- **Production-Ready**: Security headers, rate limiting, soft delete, error handling

### **Project Metrics**

| Metric | Value |
|--------|-------|
| **Total Lines of Code** | ~15,000+ |
| **Frontend Components** | 50+ React components |
| **Backend Endpoints** | 25+ REST API endpoints |
| **Database Models** | 4 Prisma models |
| **Custom Hooks** | 12+ React hooks |
| **State Stores** | 4 Zustand stores |
| **UI Component Registries** | 8 (Shadcn, Aceternity, OriginUI, etc.) |

---

## Technology Stack

### **Frontend Technologies**

| Category | Technology | Version | Purpose |
|----------|-----------|---------|---------|
| **Core Framework** | React | 19.2.3 | UI library with concurrent rendering |
| **Language** | TypeScript | 5.9.3 | Type-safe JavaScript superset |
| **Build Tool** | Vite | 7.3.1 | Next-generation frontend tooling |
| **Styling** | Tailwind CSS | 4.1.14 | Utility-first CSS framework |
| **UI Components** | Shadcn/UI | Latest | Accessible, customizable components |
| **Component Primitives** | Radix UI | Latest | Unstyled, accessible components |
| **State Management** | Zustand | 5.0.9 | Lightweight state management |
| **Routing** | React Router DOM | 7.12.0 | Declarative routing with lazy loading |
| **Drag & Drop** | @dnd-kit | 6.3.1 | Accessible drag-and-drop toolkit |
| **HTTP Client** | Fetch API | Native | HTTP requests to backend |
| **Form Handling** | React Hooks | Native | Form state and validation |
| **Icons** | Lucide React + Remixicon | 0.562.0 | Icon libraries |
| **Notifications** | Sonner | 2.0.7 | Toast notifications |
| **Charts** | Recharts | 3.2.1 | Composable charting library |
| **Date Handling** | date-fns | 4.1.0 | Date utility functions |
| **Theme** | next-themes | 0.4.6 | Dark mode support |
| **Utilities** | clsx + tailwind-merge | Latest | Class name utilities |

### **Backend Technologies**

| Category | Technology | Version | Purpose |
|----------|-----------|---------|---------|
| **Runtime** | Node.js | 18+ | JavaScript runtime environment |
| **Framework** | Express.js | 4.21.2 | Fast, unopinionated web framework |
| **Language** | TypeScript | 5.7.2 | Type-safe backend development |
| **ORM** | Prisma | 6.19.0 | Next-generation Node.js ORM |
| **Database (Dev)** | SQLite | 3.x | Lightweight SQL database |
| **Database (Prod)** | PostgreSQL | 15+ | Production-grade SQL database |
| **Storage SDK** | AWS SDK v3 | 3.966.0 | S3-compatible storage operations |
| **Security** | Helmet | 8.1.0 | Security middleware for Express |
| **CORS** | cors | 2.8.5 | Cross-Origin Resource Sharing |
| **Rate Limiting** | express-rate-limit | 7.5.1 | Request rate limiting |
| **Validation** | Joi + Zod | 17.13.3 + 3.25.76 | Schema validation libraries |
| **Encryption** | crypto (Node.js) | Native | AES-256-GCM credential encryption |
| **Authentication** | JWT + bcryptjs | 9.0.2 + 2.4.3 | Token-based auth (planned) |
| **Logging** | Winston | 3.19.0 | Flexible logging library |
| **Compression** | compression | 1.7.5 | Response compression middleware |
| **Environment** | dotenv | 16.6.1 | Environment variable management |

### **Development Tools**

| Tool | Version | Purpose |
|------|---------|---------|
| **ESLint** | 9.37.0 | JavaScript/TypeScript linting |
| **Prettier** | (Configured) | Code formatting |
| **TypeScript ESLint** | 8.45.0 | TypeScript-specific linting rules |
| **tsx** | 4.19.2 | TypeScript execution and watch mode |
| **Prisma Studio** | 6.16.3 | Database GUI |
| **Git** | 2.x | Version control |

---

## Architecture Overview

### **System Architecture**

Research Space implements a **three-tier architecture** with clear separation of concerns:

```
┌──────────────────────────────────────────────────────────────────┐
│                         PRESENTATION LAYER                       │
│                    React 19 Frontend (Vite)                      │
│                     http://localhost:5173                        │
├──────────────────────────────────────────────────────────────────┤
│                                                                  │
│  ┌─────────────────┐  ┌──────────────┐  ┌──────────────────┐  │
│  │     Pages       │  │  Components  │  │   State Stores   │  │
│  │  - Library      │  │  - FileCard  │  │  - libraryStore  │  │
│  │  - Connections  │  │  - FolderCard│  │  - uploadStore   │  │
│  │  - Agents       │  │  - DndGrid   │  │  - connectionSt. │  │
│  │  - Settings     │  │  - Modals    │  │  - themeStore    │  │
│  └────────┬────────┘  └──────┬───────┘  └────────┬─────────┘  │
│           │                  │                    │             │
│           └──────────────────┴────────────────────┘             │
│                              │                                  │
│                    ┌─────────▼─────────┐                        │
│                    │   API Clients     │                        │
│                    │  - filesApi.ts    │                        │
│                    │  - storageApi.ts  │                        │
│                    └─────────┬─────────┘                        │
└──────────────────────────────┼──────────────────────────────────┘
                               │
                     HTTP/REST API (JSON)
                     CORS-enabled requests
                               │
┌──────────────────────────────▼──────────────────────────────────┐
│                       APPLICATION LAYER                          │
│                  Express.js Backend API Server                   │
│                     http://localhost:3001                        │
├──────────────────────────────────────────────────────────────────┤
│                                                                  │
│  ┌──────────────┐  ┌──────────────┐  ┌────────────────────┐   │
│  │   Routes     │  │ Controllers  │  │     Services       │   │
│  │  /storage    │─▶│  Storage     │─▶│  - S3 Providers    │   │
│  │  /files      │─▶│  Files       │─▶│  - Encryption      │   │
│  │  /folders    │─▶│  Folders     │─▶│  - Presigned URLs  │   │
│  └──────────────┘  └──────────────┘  └────────────────────┘   │
│                                                                  │
│  ┌──────────────────────────────────────────────────────────┐  │
│  │              Middleware Layer                            │  │
│  │  - Helmet (Security Headers)                             │  │
│  │  - CORS (Cross-Origin)                                   │  │
│  │  - Rate Limiting (100 req/15min)                         │  │
│  │  - Body Parser (JSON)                                    │  │
│  │  - Compression (gzip)                                    │  │
│  │  - Validation (Joi/Zod schemas)                          │  │
│  │  - Authentication (JWT - planned)                        │  │
│  │  - Error Handling (Global handler)                       │  │
│  └──────────────────────────────────────────────────────────┘  │
└────────────┬─────────────────────────────┬─────────────────────┘
             │                             │
             │ Prisma ORM                  │ AWS SDK v3
             │ Type-safe queries           │ S3 operations
             │                             │
┌────────────▼────────────┐    ┌───────────▼─────────────────────┐
│     DATA LAYER          │    │   STORAGE LAYER                  │
│  SQLite / PostgreSQL    │    │   S3-Compatible Storage          │
├─────────────────────────┤    ├──────────────────────────────────┤
│  Models:                │    │  Providers:                      │
│  - StorageConfig        │    │  - AWS S3                        │
│  - ConfigLock           │    │  - Cloudflare R2                 │
│  - File                 │    │  - MinIO (local/self-hosted)     │
│  - Folder               │    │                                  │
│                         │    │  Operations:                     │
│  Features:              │    │  - Presigned PUT (upload)        │
│  - Soft delete          │    │  - Presigned GET (download)      │
│  - Encryption at rest   │    │  - CopyObject (duplicate)        │
│  - Hierarchical folders │    │  - DeleteObject                  │
│  - Metadata storage     │    │  - ListObjectsV2                 │
└─────────────────────────┘    └──────────────────────────────────┘
```

### **Data Flow**

#### **File Upload Flow**

```
1. User drags file to Library Page
2. Frontend: useDragAndDrop hook → uploadQueueStore
3. POST /api/files/presigned-url (filename, type, size, folderId)
4. Backend: FilesController → PresignedUrlService
5. Backend: Generate presigned PUT URL from S3 (valid 15 minutes)
6. Backend: Save file metadata to Prisma → Database
7. Backend: Return { uploadUrl, fileId, s3Key, expiresAt }
8. Frontend: Direct HTTP PUT to S3 with file binary
9. Frontend: Update upload progress (0-100%)
10. S3: Store file, return 200 OK
11. Frontend: Mark upload complete in uploadQueueStore
12. Frontend: Refresh Library Page file list
```

#### **File Download Flow**

```
1. User clicks file or "Download" in context menu
2. Frontend: POST /api/files/:id/download-url
3. Backend: Validate file exists in database (Prisma)
4. Backend: Generate presigned GET URL from S3 (valid 5 minutes)
5. Backend: Return { downloadUrl, fileName, expiresAt }
6. Frontend: window.open(downloadUrl) or direct fetch
7. S3: Serve file directly to browser
```

#### **Storage Configuration Flow**

```
1. User fills S3 credentials form on Connections Page
2. Frontend: POST /api/storage/configure { provider, credentials }
3. Backend: Validate input with Joi schema
4. Backend: Test connection → S3Provider.testConnection()
5. S3: Verify bucket access, read/write permissions
6. Backend: Encrypt credentials with AES-256-GCM
7. Backend: Save to database (StorageConfig model)
8. Backend: Create ConfigLock record (prevents re-configuration)
9. Backend: Return success status
10. Frontend: Update connectionStore, show success notification
```

### **Security Architecture**

```
┌─────────────────────────────────────────────────────────────┐
│                    Security Layers                          │
├─────────────────────────────────────────────────────────────┤
│                                                             │
│  1. Transport Layer Security                                │
│     - HTTPS in production (TLS 1.3)                         │
│     - Secure WebSocket connections                          │
│                                                             │
│  2. HTTP Security Headers (Helmet.js)                       │
│     - Content-Security-Policy                               │
│     - X-Content-Type-Options: nosniff                       │
│     - X-Frame-Options: DENY                                 │
│     - Strict-Transport-Security                             │
│                                                             │
│  3. Cross-Origin Resource Sharing (CORS)                    │
│     - Whitelist: http://localhost:5173 (dev)                │
│     - Credentials: true                                     │
│     - Methods: GET, POST, PUT, DELETE, OPTIONS              │
│                                                             │
│  4. Rate Limiting                                           │
│     - 100 requests per 15 minutes per IP                    │
│     - Applies to all /api/* endpoints                       │
│     - Prevents brute force attacks                          │
│                                                             │
│  5. Input Validation                                        │
│     - Joi schemas for request bodies                        │
│     - Zod schemas for TypeScript type safety                │
│     - SQL injection prevention (Prisma ORM)                 │
│                                                             │
│  6. Credential Encryption                                   │
│     - Algorithm: AES-256-GCM                                │
│     - Encryption at rest for S3 credentials                 │
│     - Unique IV (Initialization Vector) per record          │
│     - Authentication tag for integrity verification         │
│                                                             │
│  7. Authentication & Authorization (Planned)                │
│     - JWT tokens for session management                     │
│     - bcrypt for password hashing (10 rounds)               │
│     - Role-Based Access Control (RBAC)                      │
│                                                             │
│  8. Configuration Locking                                   │
│     - Prevent unauthorized S3 credential changes            │
│     - Admin password required for override                  │
│     - Audit trail for configuration changes                 │
│                                                             │
└─────────────────────────────────────────────────────────────┘
```

---

## Complete File Structure

### **Root Directory**

```
research-vite-app/
├── backend/                      # Backend Express.js API server
├── backup/                       # Backup files for code versions
├── docs/                         # Project-wide documentation
├── public/                       # Static assets (favicon, images)
├── src/                          # Frontend React application source
├── .github/                      # GitHub configuration and workflows
│   └── instructions/             # AI agent instruction files
├── PROJECT_OVERVIEW.md           # This file - comprehensive project documentation
├── README.md                     # Main project README with quick start
├── README.old.md                 # Backup of original Vite template README
├── components.json               # Shadcn/UI configuration (8 component registries)
├── dev.log                       # Development logs
├── eslint.config.js              # ESLint configuration for frontend
├── index.html                    # HTML entry point for Vite
├── package.json                  # Frontend dependencies and scripts
├── package-lock.json             # Frontend dependency lock file
├── tsconfig.json                 # TypeScript base configuration
├── tsconfig.app.json             # TypeScript config for application
├── tsconfig.node.json            # TypeScript config for Node.js scripts
└── vite.config.ts                # Vite build tool configuration
```

### **Frontend Structure (`/src`)**

```
src/
├── api/                          # HTTP client modules for backend API
│   ├── filesApi.ts               # Files and folders API client (500+ lines)
│   └── storageApi.ts             # Storage configuration API client (200+ lines)
│
├── assets/                       # Static assets (images, icons)
│   └── react.svg                 # React logo
│
├── components/                   # React components organized by domain
│   ├── action-buttons.tsx        # Generic action button components
│   ├── app-sidebar.tsx           # Main application sidebar navigation
│   ├── date-picker.tsx           # Date picker component
│   ├── nav-user.tsx              # User navigation menu
│   ├── chart-*.tsx               # Dashboard chart components (6 files)
│   ├── charts-extra.tsx          # Additional chart utilities
│   │
│   ├── connections/              # S3 storage configuration components
│   │   ├── AdminUnlockModal.tsx  # Admin password modal for config unlock
│   │   ├── ConnectionStatus.tsx  # Display current connection status
│   │   ├── ConnectionTestButton.tsx # Test connection button with feedback
│   │   ├── LockWarningModal.tsx  # Warning before configuration lock
│   │   ├── ProviderSelector.tsx  # S3 provider selection dropdown
│   │   └── S3ConfigForm.tsx      # Main S3 credentials form
│   │
│   ├── home/                     # Home page components
│   │   ├── StatusCard.tsx        # Dashboard status card
│   │   ├── StatusGrid.tsx        # Grid layout for status cards
│   │   └── WelcomeMessage.tsx    # Welcome banner
│   │
│   ├── layout/                   # Layout and navigation components
│   │   ├── Layout.tsx            # Main app layout with sidebar
│   │   ├── Navbar.tsx            # Top navigation bar
│   │   ├── PageContent.tsx       # Page content wrapper
│   │   └── RouteLoadingFallback.tsx # Loading skeleton for routes
│   │
│   ├── library/                  # Library page file management components
│   │   ├── BreadcrumbNavigation.tsx # Folder path breadcrumbs
│   │   ├── ContextMenu.tsx       # Right-click context menu
│   │   ├── CreateDropdown.tsx    # "New" dropdown (file/folder)
│   │   ├── CreateFolderModal.tsx # Modal for creating folders
│   │   ├── DeleteConfirmModal.tsx # Delete confirmation dialog
│   │   ├── DetailsPanel.tsx      # File/folder details sidebar
│   │   ├── DndLibraryGrid.tsx    # Drag-and-drop grid layout
│   │   ├── ExportModal.tsx       # Bulk export modal
│   │   ├── FileCard.tsx          # Individual file card component
│   │   ├── FilePreviewModal.tsx  # File preview lightbox
│   │   ├── FolderCard.tsx        # Individual folder card component
│   │   ├── GlobalDropZone.tsx    # App-level drop zone overlay
│   │   ├── LibraryControls.tsx   # Action buttons (view, sort, filter)
│   │   ├── LibrarySearchBar.tsx  # Search files/folders
│   │   ├── MoveToModal.tsx       # Move files/folders modal
│   │   ├── MultiSelectControls.tsx # Bulk action controls
│   │   ├── RenameModal.tsx       # Rename file/folder modal
│   │   ├── SelectionControls.tsx # Selection count and clear
│   │   ├── ShareModal.tsx        # Generate share links modal
│   │   ├── UploadDropdown.tsx    # Upload options dropdown
│   │   ├── UploadModal.tsx       # Upload queue modal
│   │   ├── UploadProgress.tsx    # Upload progress bar
│   │   └── UploadSteps.tsx       # Upload wizard steps
│   │
│   ├── notifications/            # Notification system
│   │   ├── NotificationBell.tsx  # Notification icon with badge
│   │   └── NotificationModal.tsx # Notification center modal
│   │
│   ├── search/                   # Search components
│   │   └── [search components]   # Global search functionality
│   │
│   ├── theme/                    # Theme switching components
│   │   └── [theme components]    # Dark/light mode toggle
│   │
│   └── ui/                       # Shadcn/UI base components
│       ├── breadcrumb.tsx        # Breadcrumb primitive
│       ├── button.tsx            # Button component
│       ├── calendar.tsx          # Calendar component
│       ├── card.tsx              # Card component
│       ├── checkbox.tsx          # Checkbox component
│       ├── collapsible.tsx       # Collapsible component
│       ├── dialog.tsx            # Dialog/Modal primitive
│       ├── dropdown-menu.tsx     # Dropdown menu component
│       ├── input.tsx             # Input field component
│       ├── label.tsx             # Form label component
│       ├── popover.tsx           # Popover component
│       ├── progress.tsx          # Progress bar component
│       ├── radio-group.tsx       # Radio group component
│       ├── scroll-area.tsx       # Scrollable area component
│       ├── select.tsx            # Select dropdown component
│       ├── separator.tsx         # Divider component
│       ├── sidebar.tsx           # Sidebar primitive
│       ├── sonner.tsx            # Toast notifications
│       ├── switch.tsx            # Toggle switch component
│       ├── SweetAlert.tsx        # Alert dialog component
│       ├── textarea.tsx          # Textarea component
│       └── tooltip.tsx           # Tooltip component
│
├── config/                       # Configuration files
│   └── navigation.ts             # Navigation menu configuration
│
├── hooks/                        # Custom React hooks
│   ├── useContextMenu.ts         # Context menu positioning and state
│   ├── useDashboardStats.ts      # Fetch and manage dashboard statistics
│   ├── useDragAndDrop.ts         # Drag-and-drop event handlers
│   ├── useLibraryNavigation.ts   # Folder navigation state management
│   ├── useLibrarySelection.ts    # File/folder selection logic
│   ├── useModalRouting.ts        # Modal state with URL synchronization
│   ├── useMultiSelect.ts         # Multi-select with shift-click support
│   ├── useNavigateToPage.ts      # Type-safe navigation helper
│   ├── usePageTitle.ts           # Dynamic page title updates
│   ├── useTheme.ts               # Theme switching logic
│   ├── useUploadSteps.ts         # Upload wizard step management
│   ├── use-mobile.ts             # Mobile breakpoint detection
│   └── use-has-primary-touch.tsx # Touch device detection
│
├── lib/                          # Utility libraries
│   └── utils.ts                  # Shared utility functions (cn, etc.)
│
├── pages/                        # Route page components
│   ├── AgentsPage.tsx            # AI Agents management page
│   ├── ConnectionsPage.tsx       # S3 configuration page
│   ├── HomePage.tsx              # Dashboard/landing page
│   ├── LibraryPage.tsx           # Main file management page
│   ├── SettingsPage.tsx          # Application settings page
│   ├── StatusPage.tsx            # System status page
│   └── index.ts                  # Page exports
│
├── router/                       # React Router configuration
│   └── index.tsx                 # Route definitions with lazy loading
│
├── services/                     # Frontend business logic
│   ├── dashboardService.ts       # Dashboard data aggregation
│   └── fileUploadService.ts      # File upload orchestration
│
├── store/                        # Zustand state stores
│   ├── connectionStore.ts        # S3 connection state
│   ├── libraryStore.ts           # Files, folders, navigation state
│   ├── libraryStore.backup.ts    # Backup of library store
│   ├── themeStore.ts             # Theme preferences
│   └── uploadQueueStore.ts       # Upload queue and progress
│
├── types/                        # TypeScript type definitions
│   ├── connection.ts             # S3 connection types
│   ├── library.ts                # File and folder types
│   ├── libraryControls.ts        # Library UI control types
│   └── libraryModals.ts          # Modal component types
│
├── App.css                       # Application-level styles
├── App.tsx                       # Root App component
├── index.css                     # Global styles and Tailwind imports
└── main.tsx                      # Application entry point
```

### **Backend Structure (`/backend`)**

```
backend/
├── data/                         # Runtime data storage (SQLite, JSON configs)
│   ├── config-lock.json          # Configuration lock status
│   ├── database.db               # SQLite database (legacy)
│   ├── files.json                # File metadata (legacy JSON storage)
│   ├── folders.json              # Folder structure (legacy JSON storage)
│   └── storage-config.json       # S3 credentials (legacy)
│
├── docs/                         # Backend-specific documentation
│   ├── example/                  # Code examples and snippets
│   ├── guide/                    # Setup and usage guides
│   │   ├── API_DOCUMENTATION.md  # Complete API endpoint reference (1200+ lines)
│   │   └── DEPLOYMENT_GUIDE.md   # Railway and production deployment guide
│   └── misc/                     # Miscellaneous documentation
│
├── prisma/                       # Prisma ORM configuration
│   ├── data/                     # Prisma-specific data files
│   ├── migrations/               # Database migration history
│   │   └── [timestamp]_[name]/   # Individual migration folders
│   ├── dev.db                    # SQLite development database
│   └── schema.prisma             # Database schema definition
│
├── src/                          # Backend source code
│   ├── config/                   # Configuration modules
│   │   └── database.config.ts    # Database connection configuration
│   │
│   ├── controllers/              # Request handlers
│   │   ├── files.controller.ts   # File operations controller (500+ lines)
│   │   ├── folders.controller.ts # Folder operations controller (300+ lines)
│   │   └── storage.controller.ts # Storage configuration controller (200+ lines)
│   │
│   ├── middleware/               # Express middleware
│   │   ├── auth.middleware.ts    # JWT authentication (planned)
│   │   ├── error.middleware.ts   # Global error handler
│   │   └── validation.middleware.ts # Joi/Zod request validation
│   │
│   ├── models/                   # Prisma client and models
│   │   ├── index.ts              # Prisma client export
│   │   └── [model].model.ts      # Model-specific utilities
│   │
│   ├── routes/                   # API route definitions
│   │   ├── files.routes.ts       # File operation routes
│   │   ├── folders.routes.ts     # Folder operation routes
│   │   └── storage.routes.ts     # Storage configuration routes
│   │
│   ├── scripts/                  # Utility scripts
│   │   └── seed.ts               # Database seeding script
│   │
│   ├── services/                 # Business logic services
│   │   ├── encryption.service.ts # AES-256-GCM credential encryption (150+ lines)
│   │   ├── minioProvider.service.ts # MinIO S3-compatible provider
│   │   ├── presignedUrl.service.ts # Presigned URL generation (200+ lines)
│   │   ├── r2Provider.service.ts # Cloudflare R2 provider
│   │   ├── s3Provider.service.ts # AWS S3 provider
│   │   └── storageProvider.service.ts # Provider abstraction layer (300+ lines)
│   │
│   ├── types/                    # TypeScript type definitions
│   │   ├── storage.types.ts      # Storage and S3 types
│   │   ├── file.types.ts         # File metadata types
│   │   └── folder.types.ts       # Folder hierarchy types
│   │
│   ├── utils/                    # Utility functions
│   │   ├── logger.ts             # Winston logger configuration
│   │   ├── routeDiscovery.ts     # Auto-discover and display API routes
│   │   └── validation.ts         # Common validation helpers
│   │
│   └── server.ts                 # Express application entry point (150+ lines)
│
├── DOCUMENTATION_QUICK_REF.md    # Quick reference for backend docs
├── README.md                     # Backend-specific README (1000+ lines)
├── README.old.md                 # Backup of previous README
├── package.json                  # Backend dependencies and scripts
├── package-lock.json             # Backend dependency lock file
├── server-test.log               # Server test logs
├── server.log                    # Production server logs
└── tsconfig.json                 # TypeScript configuration for backend
```

### **Documentation Structure (`/docs`)**

```
docs/
├── changes/                      # Change logs and bug fix records
│   ├── BUGFIX_FOLDER_DELETE.md   # Folder deletion bug fix documentation
│   └── changes.md                # General change log
│
├── guide/                        # Setup and usage guides
│   ├── API_DOCUMENTATION.md      # High-level API overview (this doc)
│   └── MINIO_SETUP_GUIDE.md      # Local MinIO setup instructions
│
├── history/                      # Historical documentation
│   └── planner_agent-1.0/        # Previous planner agent version docs
│
├── plans/                        # Domain-specific implementation plans
│   ├── backend/                  # Backend development plans
│   │   └── BACKEND_PLAN.md       # Backend implementation roadmap (✅ Complete)
│   ├── frontend/                 # Frontend development plans
│   │   ├── FRONTEND_PLAN.md      # Current frontend plan (🟡 In Progress)
│   │   ├── FRONTEND_PLAN_(v1.0).md # Version 1.0 plan
│   │   └── FRONTEND_PLAN_(v1.1).md # Version 1.1 plan
│   └── ui/                       # UI/UX design plans
│       └── (empty - UIUX_PLAN.md not yet created)
│
├── prompts/                      # AI agent prompts and templates
│   ├── Planner.prompt.md         # Planner agent prompt template
│   └── Refactoration.prompt.md   # Code refactoring prompt template
│
├── reports/                      # Status reports and analyses
│   ├── handoff/                  # Project handoff reports
│   ├── misc/                     # Miscellaneous reports
│   └── refactor/                 # Refactoring analysis reports
│
├── rules/                        # Project standards and conventions
│   ├── CODING_STANDARDS.md       # TypeScript/TSX coding conventions (1300+ lines)
│   └── PLAN_RULE.md              # PLAN file formatting and standards (270+ lines)
│
└── tasks/                        # Task lists and execution plans
    ├── BUGFIX_STRATEGY.md        # Bug fixing strategy guide
    ├── EXECUTION_SEQUENCE.md     # Task execution ordering
    ├── FRONTEND_BACKUP_RESTORE_v1.0.md # Frontend backup/restore procedures
    ├── FRONTEND_REFACTOR_v1.0.md # Frontend refactoring plan v1.0
    ├── FRONTEND_REFACTOR_v1.1.md # Frontend refactoring plan v1.1
    ├── MINOR_CHANGES.md          # Small changes and tweaks
    └── QUICK_START.md            # Fast setup instructions
```

---

## Frontend Architecture Deep Dive

### **Component Architecture**

Research Space follows **Atomic Design principles** with a clear component hierarchy:

```
📦 Atomic Design Levels
├── 🔴 Atoms (src/components/ui/)
│   └── Basic building blocks: Button, Input, Label, Switch, Checkbox
│
├── 🟠 Molecules (src/components/library/)
│   └── Simple combinations: FileCard, FolderCard, SearchBar, UploadProgress
│
├── 🟡 Organisms (src/components/library/)
│   └── Complex groups: DndLibraryGrid, BreadcrumbNavigation, ContextMenu
│
├── 🟢 Templates (src/components/layout/)
│   └── Page layouts: Layout, Navbar, Sidebar, PageContent
│
└── 🔵 Pages (src/pages/)
    └── Full routes: LibraryPage, ConnectionsPage, HomePage, SettingsPage
```

### **State Management Strategy**

```typescript
// Store Architecture
┌─────────────────────────────────────────────────────────┐
│                  Zustand Stores                         │
├─────────────────────────────────────────────────────────┤
│                                                         │
│  libraryStore (src/store/libraryStore.ts)              │
│  - files: FileMetadata[]                               │
│  - folders: Folder[]                                   │
│  - currentFolderId: string | null                      │
│  - selectedItems: Set<string>                          │
│  - viewMode: 'grid' | 'list'                           │
│  - sortBy: 'name' | 'date' | 'size'                    │
│  - Actions: setFiles, addFile, deleteFile, moveFile    │
│                                                         │
│  connectionStore (src/store/connectionStore.ts)        │
│  - status: ConnectionStatus                            │
│  - provider: StorageProvider | null                    │
│  - isConfigured: boolean                               │
│  - isLocked: boolean                                   │
│  - Actions: setStatus, configure, testConnection       │
│                                                         │
│  uploadQueueStore (src/store/uploadQueueStore.ts)      │
│  - queue: UploadTask[]                                 │
│  - isUploading: boolean                                │
│  - totalProgress: number                               │
│  - Actions: addToQueue, updateProgress, removeFromQueue│
│                                                         │
│  themeStore (src/store/themeStore.ts)                  │
│  - theme: 'light' | 'dark' | 'system'                  │
│  - Actions: setTheme, toggleTheme                      │
│                                                         │
└─────────────────────────────────────────────────────────┘
```

### **Routing Configuration**

```typescript
// React Router v7 with Lazy Loading
routes = [
  {
    path: '/',
    element: <Layout />,
    children: [
      { index: true, element: <HomePage /> },  // Eager-loaded
      { path: 'library', element: lazy(() => <LibraryPage />) },
      { path: 'library/folders/:folderId', element: lazy(() => <LibraryPage />) },
      { path: 'connections', element: lazy(() => <ConnectionsPage />) },
      { path: 'agents', element: lazy(() => <AgentsPage />) },
      { path: 'status', element: lazy(() => <StatusPage />) },
      { path: 'settings', element: lazy(() => <SettingsPage />) },
    ]
  }
]
```

---

## Backend Architecture Deep Dive

### **Express Middleware Stack**

```javascript
// Middleware execution order (server.ts)
┌────────────────────────────────────────────┐
│  1. Helmet.js - Security Headers          │
├────────────────────────────────────────────┤
│  2. CORS - Cross-Origin Configuration     │
├────────────────────────────────────────────┤
│  3. Body Parser - JSON & URL-encoded      │
├────────────────────────────────────────────┤
│  4. Compression - gzip Response            │
├────────────────────────────────────────────┤
│  5. Rate Limiter - 100 req/15min          │
├────────────────────────────────────────────┤
│  6. Request Logger - Winston Logging       │
├────────────────────────────────────────────┤
│  7. Route Handlers - /api/storage, etc.   │
├────────────────────────────────────────────┤
│  8. 404 Handler - Not Found Middleware    │
├────────────────────────────────────────────┤
│  9. Error Handler - Global Error Handling │
└────────────────────────────────────────────┘
```

### **Service Layer Architecture**

```typescript
// Service Layer Pattern
┌─────────────────────────────────────────────────────┐
│           StorageProviderService                    │
│  (Provider Abstraction - Factory Pattern)           │
├─────────────────────────────────────────────────────┤
│  + createProvider(provider: string): IStorageProvider
│  + getProvider(): IStorageProvider                  │
└───────────────────┬─────────────────────────────────┘
                    │ implements
        ┌───────────┴───────────────────┐
        │                               │
┌───────▼────────┐  ┌──────────────┐  ┌▼───────────────┐
│ S3Provider     │  │ R2Provider   │  │ MinIOProvider  │
│                │  │              │  │                │
│ + upload()     │  │ + upload()   │  │ + upload()     │
│ + download()   │  │ + download() │  │ + download()   │
│ + delete()     │  │ + delete()   │  │ + delete()     │
│ + copy()       │  │ + copy()     │  │ + copy()       │
│ + test()       │  │ + test()     │  │ + test()       │
└────────────────┘  └──────────────┘  └────────────────┘
```

### **Controller → Service → Model Flow**

```typescript
// Example: File Upload Flow
┌──────────────────────────────────────────────────────┐
│  1. Client Request                                   │
│     POST /api/files/presigned-url                    │
│     Body: { fileName, fileType, fileSize, folderId } │
└───────────────────┬──────────────────────────────────┘
                    │
┌───────────────────▼──────────────────────────────────┐
│  2. Route Handler (files.routes.ts)                  │
│     router.post('/presigned-url', ...)               │
└───────────────────┬──────────────────────────────────┘
                    │
┌───────────────────▼──────────────────────────────────┐
│  3. Validation Middleware                            │
│     - Validate request body with Joi schema          │
│     - Check file size limits                         │
└───────────────────┬──────────────────────────────────┘
                    │
┌───────────────────▼──────────────────────────────────┐
│  4. Controller (FilesController.getPresignedUploadUrl)│
│     - Extract request data                           │
│     - Generate unique s3Key                          │
└───────────────────┬──────────────────────────────────┘
                    │
┌───────────────────▼──────────────────────────────────┐
│  5. Service (PresignedUrlService.generateUploadUrl)  │
│     - Get S3 provider instance                       │
│     - Generate presigned PUT URL (15 min expiry)     │
└───────────────────┬──────────────────────────────────┘
                    │
┌───────────────────▼──────────────────────────────────┐
│  6. Model (Prisma Client)                            │
│     - prisma.file.create({ s3Key, name, size, ... }) │
│     - Save file metadata to database                 │
└───────────────────┬──────────────────────────────────┘
                    │
┌───────────────────▼──────────────────────────────────┐
│  7. Response to Client                               │
│     { uploadUrl, fileId, s3Key, expiresAt }          │
└──────────────────────────────────────────────────────┘
```

---

## Database Schema

### **Prisma Models**

```prisma
// schema.prisma

// Storage Configuration Model
model StorageConfig {
  id            String   @id @default(uuid())
  provider      String   // "aws-s3", "cloudflare-r2", "minio"
  encryptedData String   // AES-256-GCM encrypted credentials
  iv            String   // Initialization Vector
  authTag       String   // Authentication Tag
  isLocked      Boolean  @default(false)
  createdAt     DateTime @default(now())
  updatedAt     DateTime @updatedAt
  
  lock          ConfigLock?
}

// Configuration Lock Model
model ConfigLock {
  id              String   @id @default(uuid())
  configurationId String   @unique
  lockedAt        DateTime @default(now())
  lockedBy        String
  reason          String
  canOverride     Boolean  @default(false)
  
  config          StorageConfig @relation(fields: [configurationId], references: [id], onDelete: Cascade)
}

// File Metadata Model
model File {
  id        String    @id @default(uuid())
  name      String
  size      Int       // Bytes
  type      String    // MIME type
  s3Key     String    @unique  // S3 object key
  folderId  String?   // Parent folder (null = root)
  starred   Boolean   @default(false)
  createdAt DateTime  @default(now())
  updatedAt DateTime  @updatedAt
  deletedAt DateTime? // Soft delete timestamp
  
  folder    Folder?   @relation(fields: [folderId], references: [id], onDelete: SetNull)
  
  @@index([folderId])
  @@index([deletedAt])
  @@index([starred])
}

// Folder Hierarchy Model
model Folder {
  id        String    @id @default(uuid())
  name      String
  parentId  String?   // Parent folder (null = root)
  path      String    // Full path (e.g., "/Documents/Work")
  starred   Boolean   @default(false)
  createdAt DateTime  @default(now())
  updatedAt DateTime  @updatedAt
  deletedAt DateTime? // Soft delete timestamp
  
  parent    Folder?   @relation("FolderHierarchy", fields: [parentId], references: [id], onDelete: Cascade)
  children  Folder[]  @relation("FolderHierarchy")
  files     File[]
  
  @@index([parentId])
  @@index([starred])
  @@index([deletedAt])
}
```

### **Database Relationships**

```
StorageConfig 1───1 ConfigLock
              (one-to-one)

Folder 1───∞ Folder
       (self-referencing parent-child)

Folder 1───∞ File
       (one-to-many)
```

---

## API Architecture

### **REST API Endpoints**

| Method | Endpoint | Purpose | Status |
|--------|----------|---------|--------|
| **Health Check** ||||
| GET | `/health` | Server health check | ✅ |
| **Storage Management** ||||
| GET | `/api/storage/status` | Get configuration status | ✅ |
| POST | `/api/storage/configure` | Configure S3 provider | ✅ |
| POST | `/api/storage/test` | Test connection | ✅ |
| DELETE | `/api/storage/lock` | Remove config lock (admin) | ✅ |
| **File Operations** ||||
| POST | `/api/files/presigned-url` | Get upload URL | ✅ |
| POST | `/api/files/:id/download-url` | Get download URL | ✅ |
| POST | `/api/files/:id/preview-url` | Get preview URL | ✅ |
| GET | `/api/files/list` | List files (paginated) | ✅ |
| GET | `/api/files/stats` | Library statistics | ✅ |
| GET | `/api/files/:id` | Get file metadata | ✅ |
| PUT | `/api/files/:id` | Update file metadata | ✅ |
| DELETE | `/api/files/:id` | Delete file (soft) | ✅ |
| POST | `/api/files/bulk-delete` | Bulk delete files | ✅ |
| POST | `/api/files/bulk-move` | Bulk move files | ✅ |
| **Context Menu Operations** ||||
| POST | `/api/files/:id/share` | Generate share link | ✅ |
| POST | `/api/files/:id/duplicate` | Duplicate file (S3 copy) | ✅ |
| PUT | `/api/files/:id/star` | Toggle star/favorite | ✅ |
| **Folder Operations** ||||
| POST | `/api/folders` | Create folder | ✅ |
| GET | `/api/folders` | List folders | ✅ |
| GET | `/api/folders/:id` | Get folder details | ✅ |
| GET | `/api/folders/:id/contents` | Get folder contents | ✅ |
| GET | `/api/folders/:id/breadcrumb` | Get folder path | ✅ |
| PUT | `/api/folders/:id` | Update folder (rename) | ✅ |
| DELETE | `/api/folders/:id` | Delete folder (recursive) | ✅ |

For complete API documentation with request/response examples, see:

- **High-level overview**: [`docs/guide/API_DOCUMENTATION.md`](docs/guide/API_DOCUMENTATION.md)
- **Detailed reference**: [`backend/docs/guide/API_DOCUMENTATION.md`](backend/docs/guide/API_DOCUMENTATION.md)

---

## State Management

### **Zustand Store Patterns**

```typescript
// Example: libraryStore.ts
import { create } from 'zustand';

interface LibraryStore {
  // State
  files: FileMetadata[];
  folders: Folder[];
  currentFolderId: string | null;
  selectedItems: Set<string>;
  
  // Actions
  setFiles: (files: FileMetadata[]) => void;
  addFile: (file: FileMetadata) => void;
  deleteFile: (fileId: string) => void;
  navigateToFolder: (folderId: string | null) => void;
  toggleSelection: (itemId: string) => void;
  clearSelection: () => void;
}

export const useLibraryStore = create<LibraryStore>((set) => ({
  // Initial state
  files: [],
  folders: [],
  currentFolderId: null,
  selectedItems: new Set(),
  
  // Action implementations
  setFiles: (files) => set({ files }),
  addFile: (file) => set((state) => ({ 
    files: [...state.files, file] 
  })),
  deleteFile: (fileId) => set((state) => ({
    files: state.files.filter(f => f.id !== fileId)
  })),
  navigateToFolder: (folderId) => set({ 
    currentFolderId: folderId,
    selectedItems: new Set() // Clear selection on navigation
  }),
  toggleSelection: (itemId) => set((state) => {
    const newSelection = new Set(state.selectedItems);
    if (newSelection.has(itemId)) {
      newSelection.delete(itemId);
    } else {
      newSelection.add(itemId);
    }
    return { selectedItems: newSelection };
  }),
  clearSelection: () => set({ selectedItems: new Set() }),
}));
```

---

## Component Hierarchy

### **Library Page Component Tree**

```
LibraryPage
├── GlobalDropZone
├── BreadcrumbNavigation
├── LibrarySearchBar
├── LibraryControls
│   ├── CreateDropdown
│   ├── UploadDropdown
│   ├── ViewModeToggle
│   └── SortDropdown
├── SelectionControls (if items selected)
│   └── MultiSelectControls
├── DndLibraryGrid
│   ├── FolderCard (multiple)
│   │   ├── Checkbox
│   │   ├── FolderIcon
│   │   ├── FolderName
│   │   ├── ItemCount
│   │   └── ContextMenu
│   └── FileCard (multiple)
│       ├── Checkbox
│       ├── FileThumbnail
│       ├── FileName
│       ├── FileSize
│       ├── FileDate
│       └── ContextMenu
├── DetailsPanel (sidebar)
│   ├── FileThumbnail
│   ├── FileMetadata
│   └── ActionButtons
└── Modals (conditional rendering)
    ├── CreateFolderModal
    ├── RenameModal
    ├── DeleteConfirmModal
    ├── MoveToModal
    ├── ShareModal
    ├── UploadModal
    │   ├── UploadSteps
    │   └── UploadProgress
    └── FilePreviewModal
```

---

## Development Status

### **Implementation Progress**

| Domain | Status | Phases Complete | Notes |
|--------|--------|-----------------|-------|
| **Backend** | ✅ Complete | 2/2 | All API endpoints implemented |
| **Frontend** | 🟡 In Progress | 2/5 | Library UI done, S3 integration in progress |
| **UI/UX** | ❌ Not Started | 0/0 | No UIUX_PLAN.md created yet |

### **Feature Status**

| Feature | Status | Notes |
|---------|--------|-------|
| S3 Configuration | ✅ | Multi-provider support |
| File Upload (Presigned) | ✅ | Direct-to-S3 uploads |
| File Download (Presigned) | ✅ | Time-limited access |
| Folder Hierarchy | ✅ | Unlimited nesting |
| Drag-and-Drop | ✅ | @dnd-kit implementation |
| Multi-Select | ✅ | Shift-click range selection |
| Context Menu | ✅ | Right-click operations |
| Search | 🟡 | UI implemented, backend pending |
| Bulk Operations | ✅ | Move and delete |
| File Preview | 🟡 | Modal implemented, preview generation pending |
| Share Links | 🟡 | Endpoint exists, UI integration pending |
| Authentication | ❌ | JWT planned |
| User Management | ❌ | Planned |
| Permissions/RBAC | ❌ | Planned |

---

## Documentation Index

### **Core Documentation**

- 📘 **[README.md](README.md)** - Main project README with quick start guide
- 📖 **[PROJECT_OVERVIEW.md](PROJECT_OVERVIEW.md)** - This file - comprehensive project documentation
- 🔧 **[backend/README.md](backend/README.md)** - Complete backend guide with detailed examples

### **API Documentation**

- 🚀 **[docs/guide/API_DOCUMENTATION.md](docs/guide/API_DOCUMENTATION.md)** - High-level API overview
- 📚 **[backend/docs/guide/API_DOCUMENTATION.md](backend/docs/guide/API_DOCUMENTATION.md)** - Detailed endpoint reference (1200+ lines)

### **Setup Guides**

- ⚡ **[docs/tasks/QUICK_START.md](docs/tasks/QUICK_START.md)** - Fast setup instructions
- 🐳 **[docs/guide/MINIO_SETUP_GUIDE.md](docs/guide/MINIO_SETUP_GUIDE.md)** - Local MinIO S3 setup
- 🚀 **[backend/docs/guide/DEPLOYMENT_GUIDE.md](backend/docs/guide/DEPLOYMENT_GUIDE.md)** - Railway deployment guide

### **Development Standards**

- 📏 **[docs/rules/CODING_STANDARDS.md](docs/rules/CODING_STANDARDS.md)** - TypeScript/TSX conventions (1300+ lines)
- 📝 **[docs/rules/PLAN_RULE.md](docs/rules/PLAN_RULE.md)** - Project planning standards (270+ lines)

### **Implementation Plans**

- 🎨 **[docs/plans/frontend/FRONTEND_PLAN.md](docs/plans/frontend/FRONTEND_PLAN.md)** - Frontend roadmap (🟡 Phase 2/5)
- ⚙️ **[docs/plans/backend/BACKEND_PLAN.md](docs/plans/backend/BACKEND_PLAN.md)** - Backend roadmap (✅ Complete)

### **AI Agent Instructions**

- 🤖 **[.github/instructions/planner.instructions.md](.github/instructions/planner.instructions.md)** - Project coordination
- 🎨 **[.github/instructions/frontend.instructions.md](.github/instructions/frontend.instructions.md)** - Frontend development
- ⚙️ **[.github/instructions/backend.instructions.md](.github/instructions/backend.instructions.md)** - Backend development
- 🎭 **[.github/instructions/ui-ux.instructions.md](.github/instructions/ui-ux.instructions.md)** - Design system

---

## Setup Instructions

### **Prerequisites**

- **Node.js** 18+ with npm 9+
- **Git** for version control
- **S3-compatible storage** account (optional):
  - AWS S3 account
  - Cloudflare R2 account
  - Local MinIO instance ([setup guide](docs/guide/MINIO_SETUP_GUIDE.md))

### **Quick Setup**

```bash
# 1. Clone repository
git clone https://github.com/PRATS-gits/research-vite-app.git
cd research-vite-app

# 2. Install frontend dependencies
npm install

# 3. Install backend dependencies
cd backend
npm install

# 4. Setup backend environment
cp .env.example .env
# Edit .env with your configuration

# 5. Setup database
npx prisma generate
npx prisma migrate dev

# 6. Start development servers
# Terminal 1 - Backend
npm run dev

# Terminal 2 - Frontend (from root)
cd ..
npm run dev
```

**Access the application:**

- Frontend: `http://localhost:5173`
- Backend API: `http://localhost:3001`

For complete setup instructions, see [README.md](README.md#quick-start)

---

## Development Workflow

### **Git Workflow**

```bash
# Create feature branch
git checkout -b feature/your-feature-name

# Make changes and commit
git add .
git commit -m "feat: add new feature"

# Push and create PR
git push origin feature/your-feature-name
```

### **Commit Convention**

Follow [Conventional Commits](https://www.conventionalcommits.org/):

- `feat:` New features
- `fix:` Bug fixes
- `docs:` Documentation changes
- `refactor:` Code refactoring
- `test:` Adding tests
- `chore:` Maintenance tasks
- `style:` Code style changes
- `perf:` Performance improvements

### **Testing Workflow**

```bash
# Frontend
npm run lint        # Run ESLint
npm run type-check  # TypeScript checking
npm run test        # Unit tests (planned)

# Backend
cd backend
npm run lint        # Run ESLint
npm run test        # Jest tests
npm run test:coverage # Coverage report
```

---

**Last Updated:** January 11, 2026  
**Maintained by:** Research Space Development Team  
**Version:** 2.1
