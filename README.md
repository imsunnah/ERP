# OSAKA Group ERP - Frontend Application

A modern, enterprise-grade Enterprise Resource Planning (ERP) platform built with **Next.js 16+ (App Router & Turbopack)**, **TypeScript**, **Tailwind CSS**, **shadcn/ui**, and **Google Inter Font**.

---

## 🌟 Table of Contents

- [Overview & Architecture](#-overview--architecture)
- [Navigation Structure](#-navigation-structure)
- [System Modules & Features](#-system-modules--features)
  - [1. Overview Dashboard](#1-overview-dashboard-)
  - [2. Production Management](#2-production-management-production)
  - [3. HR Management](#3-hr-management-hr)
  - [4. Payroll Management](#4-payroll-management-payroll)
  - [5. Procurement Management](#5-procurement-management-procurement)
  - [6. Inventory Management](#6-inventory-management-inventory)
  - [7. System Configuration](#7-system-configuration-settings)
  - [8. Authentication & Security](#8-authentication--security-auth)
- [Decoupled 4-Layer Architecture](#-decoupled-4-layer-architecture)
- [API Configuration & Reverse Proxy](#-api-configuration--reverse-proxy)
- [Design System & UI Guidelines](#-design-system--ui-guidelines)
- [Getting Started](#-getting-started)
- [Directory Structure](#-directory-structure)
- [Available Documentation](#-available-documentation)

---

## 🏢 Overview & Architecture

OSAKA Group ERP is designed for scalability, zero content layout shifts (CLS), and instantaneous transitions between isolated mock datasets and live microservices.

### Key Architectural Pillars:
- **Strict Module-Aligned Feature Hierarchy**: All feature logic resides within `src/features/<module>/<feature_name>/`, mirroring `src/app/(dashboard)/<module>/<feature_name>/`.
- **Decoupled Data Flow**: UI components consume custom React hooks, which consume service contracts (`I<Feature>Service`). Live APIs or mock datasets are swappable via a single exported instance.
- **Fixed Viewport Shell**: Sidebar (`w-64`) and Topbar (`h-16`) remain locked in viewport; only `<main>` scrolls vertically.
- **Collapsible Grouped Sidebar**: Navigation is organized into module groups that can be expanded or collapsed, with the active item highlighted by a white pill on the light gray sidebar.
- **Resilient IAM Session Management**: Automatic 401 token refresh retries with single-use OAuth2 refresh token rotation.
- **Zero Hardcoded Fake Values**: Animated `<Skeleton />` loaders during data fetch and clean, clear empty states.
- **Toast Notifications**: Built-in notifications using shadcn/ui Sonner (`toast.success`, `toast.error`, `toast.warning`).

---

## 🧭 Navigation Structure

The sidebar (`src/config/navigation.ts`) is divided into the following collapsible groups:

| Group | Items |
| :--- | :--- |
| **Overview** | Overview |
| **Production Management** | Production Planning, Production Requisition, Assembly Line, Production History |
| **HR Management** | Employee Management, Attendance & Leave, Movement Management, Holiday Set-up |
| **Payroll Management** | Payroll Period, Salary Import, Salary Calculation, Advance Salary, Payslip |
| **Procurement Management** | Procurement |
| **Inventory Management** | Inventory Stock, Production Requisition, Product Receive Request, Transaction History |
| **System Configuration** | User Management, Production Setup, General Setup |

---

## 📦 System Modules & Features

> **Note:** Sections marked with ⚠️ were added from the sidebar menu and describe the *intended* scope based on menu labels. Verify routes, features and mock/live status against the actual implementation and update accordingly.

### 1. Overview Dashboard (`/`) ⚠️

| Sub-Module | Route | Features & Capabilities |
| :--- | :--- | :--- |
| **Overview** | `/` | Landing dashboard with cross-module summary metrics and quick access to key ERP areas. |

---

### 2. Production Management (`/production`) ⚠️

| Sub-Module | Route | Features & Capabilities |
| :--- | :--- | :--- |
| **Production Planning** | `/production/planning` | Production schedules, targets, and order planning. |
| **Production Requisition** | `/production/requisition` | Material and resource requests raised against production orders. |
| **Assembly Line** | `/production/assembly-line` | Assembly line tracking, status, and output monitoring. |
| **Production History** | `/production/history` | Historical production records and logs. |

---

### 3. HR Management (`/hr`)

| Sub-Module | Route | Features & Capabilities |
| :--- | :--- | :--- |
| **Employee Management** | `/hr/employees` | Grid & List views with smooth view toggles, multi-parameter search/filters (Department, Designation, Status), dynamic statistics cards, and printable Employee Summary Sheet. |
| ↳ Employee Registration | `/hr/employees/new` | Multi-step onboarding form capturing personal details, job roles, emergency contacts, bank details, and document uploads. |
| ↳ Employee Profile | `/hr/employees/[id]` | Comprehensive employee dossier displaying employment history, emergency contacts, job information, and edit dialogs. |
| **Attendance & Leave** | `/hr/attendance` | Attendance logs and leave requisitions tabs, interactive date range popover, metric summary cards (Present, Absent, Late, On Leave), and printable Attendance & Leave statements. |
| **Movement Management** | `/hr/movement` | Official and outside-factory movement logs with time pickers, status transitions (`Outside`, `Completed`), and Add/Edit modals. |
| **Holiday Set-up** | `/hr/holiday` | Calendar & List views for institutional holidays, department-specific or company-wide holiday assignment, and yearly summary statistics. |

---

### 4. Payroll Management (`/payroll`)

| Sub-Module | Route | Features & Capabilities |
| :--- | :--- | :--- |
| **Payroll Period** | `/payroll/period` | Monthly/annual payroll cycle management, lock/unlock cycle actions, detailed salary sheet generation, and printable Annual Payroll Summary. |
| **Salary Import** | `/payroll/import` | Bulk attendance and overtime CSV/Excel data import with validation diagnostics and batch parsing. |
| **Salary Calculation** | `/payroll/calculation` | Automated gross salary, deductions (late, absent, tax, provident fund), allowances, overtime, and net pay computation. |
| **Advance Salary** | `/payroll/advance` | Employee salary advance requests, approval workflows, repayment installment schedules, and printable statements. |
| **Payslip** | `/payroll/payslip` | Employee payslip generator with interactive metric cards (featuring dynamic terracotta hover animations and facet watermarks), and 1-click printable Payslip Invoice modals. |

---

### 5. Procurement Management (`/procurement`) ⚠️

| Sub-Module | Route | Features & Capabilities |
| :--- | :--- | :--- |
| **Procurement** | `/procurement` | Purchase requests, supplier orders, and procurement tracking. |

---

### 6. Inventory Management (`/inventory`) ⚠️

| Sub-Module | Route | Features & Capabilities |
| :--- | :--- | :--- |
| **Inventory Stock** | `/inventory/stock` | Current stock levels across items and locations. |
| **Production Requisition** | `/inventory/requisition` | Store-side fulfilment of material requests raised by Production. |
| **Product Receive Request** | `/inventory/receive-request` | Incoming product receipt requests and confirmations. |
| **Transaction History** | `/inventory/transactions` | Stock movement ledger (receipts, issues, adjustments). |

---

### 7. System Configuration (`/settings`)

| Sub-Module | Route | Features & Capabilities |
| :--- | :--- | :--- |
| **User Management** | `/settings/user-management` | **3-in-1 Unified Administration Suite**:<br>• **List of User**: User accounts, avatars, designations, roles, created details, and status.<br>• **Role Management**: Role definitions with assigned user counts and status.<br>• **Permission Management**: Granular matrix across all ERP modules (`View`, `Create`, `Edit`, `Delete`, `Export`, `Approve`, `All`).<br>• `+ Create User` and `+ Create Role` dialogs. |
| **Production Setup** | `/settings/production-setup` | Assembly lines, machine configurations, and production parameters. |
| **General Setup** | `/settings/general-setup` | **9-Tab Enterprise Configuration Engine**:<br>• Designation • Department • Shift • Bank • Branch • Country • District • Division • Thana.<br>Includes full CRUD modals, search, and data tables. |

---

### 8. Authentication & Security (`/auth`)

- **OAuth2 Password Grant Flow**: Direct integration with IAM microservice (`/iam/api/v1/auth/token`).
- **Token Rotation**: 2-hour access tokens (`7200s`) and 15-day refresh tokens with automated background silent refresh.
- **Route Guard**: Client-side route interception (`AuthGuard`) preventing unauthenticated access to protected dashboard paths.
- **Brand Login Banner**: Dark glassmorphic presentation banner with subtle ambient glows and enterprise compliance badges.

---

## 🔄 Decoupled 4-Layer Architecture

To ensure strict separation of concerns, the application follows a 4-layer flow:

```text
Next.js Page (page.tsx)
      │
      ▼
Feature Components (<Feature>Table, <Feature>Modal)
      │
      ▼
Custom Hook (use<Feature>)
      │
      ▼
Service Interface (I<Feature>Service)
      │
      ├── Mock Service (Mock<Feature>Service)  ──► Isolated Mock Datasets
      └── API Service  (Api<Feature>Service)   ──► Live Microservices via Reverse Proxy
```

- UI components and pages **never** make direct `fetch` calls or touch raw mock arrays.
- Swapping between mock data and live REST APIs only requires updating the single exported service instance in `src/features/<module>/<feature_name>/services/<feature_name>-service.ts`.

---

## 🌐 API Configuration & Reverse Proxy

To prevent browser Cross-Origin Resource Sharing (CORS) preflight issues during local development, Next.js acts as a **reverse proxy** via [`next.config.ts`](file:///d:/wamp64/new-git-projects/osaka-group-erp-frontend/next.config.ts):

| Proxy Endpoint | Target Microservice Destination |
| :--- | :--- |
| `/hrm-proxy/api/v1/:path*` | `${API_HOST}/hrm/api/v1/:path*` |
| `/iam-proxy/api/v1/:path*` | `${API_HOST}/iam/api/v1/:path*` |

> ⚠️ If Production, Procurement, or Inventory use their own microservices, add their proxy entries here (for example `/production-proxy`, `/procurement-proxy`, `/inventory-proxy`).

### Configuring the API Host

Create a `.env.local` file in the root directory:

```env
# Central API Server Host (Target IP / Domain)
NEXT_PUBLIC_API_HOST=http://192.168.100.26:11111
```

---

## 🎨 Design System & UI Guidelines

- **Typography**: Google **Inter** font (`font-sans`).
- **Brand Logo Color**: Vibrant Corporate Red (`#E5252A`).
- **Primary Accent**: Terracotta (`#D95338`, hover `#C8442B`) for primary action buttons, active tab pills, and checked matrix boxes.
- **Canvas Background**: Light enterprise gray (`bg-[#F8F9FA]`).
- **Sidebar**: Light gray surface with collapsible group headers (muted small-caps style labels), `lucide-react` icons per item, and a white rounded pill highlighting the active route.
- **Card Containers**: Pure white (`bg-white`), rounded corners (`rounded-2xl`), subtle border (`border-[#EAECF0]`), and shadow (`shadow-2xs`).
- **Status Pills**:
  - `Active` / `Permanent` / `Present`: Green pill (`bg-[#E6F4EA] text-[#1E8E3E]`)
  - `Inactive` / `Terminated` / `Absent`: Red pill (`bg-[#FEE2E2] text-[#DC2626]`)
  - `Late` / `Outside`: Amber pill (`bg-[#FEF3C7] text-[#D97706]`)
- **Iconography**: Clean, consistent vector icons from `lucide-react`.

---

## 🚀 Getting Started

### Prerequisites
- Node.js 18.18+ or 20+
- npm, yarn, or pnpm

### 1. Install Dependencies
```bash
npm install
```

### 2. Configure Environment Variables
Create a `.env.local` file:
```env
NEXT_PUBLIC_API_HOST=http://192.168.100.26:11111
```

### 3. Run Development Server
```bash
npm run dev
```
Open [http://localhost:3000](http://localhost:3000) in your browser.

### 4. Build for Production
```bash
npm run build
npm run start
```

---

## 📁 Directory Structure

```text
src/
├── app/                              # Next.js App Router
│   ├── (auth)/                       # Authentication routes (/login, /register)
│   ├── (dashboard)/                  # Protected ERP dashboard routes
│   │   ├── production/               # Production routes (planning, requisition, assembly-line, history)
│   │   ├── hr/                       # HR routes (employees, attendance, movement, holiday)
│   │   ├── payroll/                  # Payroll routes (period, import, calculation, advance, payslip)
│   │   ├── procurement/              # Procurement routes
│   │   ├── inventory/                # Inventory routes (stock, requisition, receive-request, transactions)
│   │   ├── settings/                 # System config routes (user-management, production-setup, general-setup)
│   │   └── layout.tsx                # AppShell viewport locking wrapper
│   └── api/                          # Next.js internal API handlers
├── components/                       # Shared global components
│   ├── layout/                       # Sidebar, Topbar, AppShell, MobileNav
│   ├── ui/                           # shadcn/ui accessible primitives (Button, Dialog, Skeleton, Sonner)
│   └── auth/                         # AuthGuard client route protection
├── config/                           # Central configuration
│   ├── api.config.ts                 # API microservice endpoints & base URLs
│   └── navigation.ts                 # Sidebar navigation structure & icons
├── features/                         # Module-aligned business logic
│   ├── auth/                         # Auth state, JWT utils, login/register forms
│   ├── production/                   # Production sub-features (planning, requisition, assembly-line, history)
│   ├── hr/                           # HR sub-features (employees, attendance, movement, holiday)
│   ├── payroll/                      # Payroll sub-features (period, import, calculation, advance, payslip)
│   ├── procurement/                  # Procurement sub-features
│   ├── inventory/                    # Inventory sub-features (stock, requisition, receive-request, transactions)
│   └── settings/                     # Settings sub-features (user-management, production-setup, general-setup)
│       └── <feature_name>/
│           ├── components/           # Feature UI components & modals
│           ├── hooks/                # Custom React state hooks (use<Feature>)
│           ├── services/             # Interfaces, API services, & contracts
│           ├── types/                # Domain TypeScript interfaces
│           ├── schemas/              # Zod validation schemas
│           └── mock/                 # Isolated mock datasets
└── lib/                              # Shared utilities & API client helpers
```

---

## 📖 Available Documentation

- 📜 **[Architecture & Conventions Guide (AGENTS.md)](file:///d:/wamp64/new-git-projects/osaka-group-erp-frontend/AGENTS.md)** — Architectural conventions, skeleton rules, and token standards.
- 🤖 **[AI Agent System Instructions Prompt](file:///d:/wamp64/new-git-projects/osaka-group-erp-frontend/docs/AGENT_PROMPT.md)** (`docs/AGENT_PROMPT.md`) — Reusable prompt addon to enforce this exact architecture when prompting AI agents.
- 🔄 **[Data Flow Architecture Guide](file:///d:/wamp64/new-git-projects/osaka-group-erp-frontend/docs/DATA_FLOW_GUIDE.md)** (`docs/DATA_FLOW_GUIDE.md`) — Guide explaining how data flows across View, Hooks, Services, and APIs.
- 📖 **[Frontend Guidelines Handbook](file:///d:/wamp64/new-git-projects/osaka-group-erp-frontend/docs/FRONTEND_GUIDELINES.md)** (`docs/FRONTEND_GUIDELINES.md`) — Step-by-step tutorials for adding new pages or ERP sub-features.
