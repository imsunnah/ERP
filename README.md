Homepage
Primary navigation
Project
O
osaka-frontend

Pinned

Manage

Plan

Code
Merge requests
0
Repository
Branches
Commits
Tags
Repository graph
Compare revisions
Snippets

Build

Secure

Deploy

Operate

Monitor

Analyze

Help

Collapse sidebar
php
osaka-frontend
Repository
Files
Search files (*.vue, *.rb...)
f
do
‎cs‎
pub
‎lic‎
scr
‎ipts‎
s
‎rc‎
.giti
‎gnore‎
AGEN
‎TS.md‎
CLAU
‎DE.md‎
Jenki
‎nsfile‎
READ
‎ME.md‎
e
‎nv‎
err
‎.txt‎
eslint.c
‎onfig.mjs‎
next.co
‎nfig.ts‎
package-
‎lock.json‎
packag
‎e.json‎
postcss.c
‎onfig.mjs‎
tsc_err
‎ors.txt‎
tsc_out
‎put.txt‎
tsconf
‎ig.json‎
osaka-frontend
README.md
README.md
user avatar
Update README.md
Most recent commit.
Moazzem007 authored 1 month ago
e30904dd
README.md
11.76 KiB
# OSAKA Group ERP - Frontend Application

A modern, enterprise-grade Enterprise Resource Planning (ERP) platform built with **Next.js 16+ (App Router & Turbopack)**, **TypeScript**, **Tailwind CSS**, **shadcn/ui**, and **Google Inter Font**.

---

## 🌟 Table of Contents

- [Overview & Architecture](#-overview--architecture)
- [System Modules & Features](#-system-modules--features)
  - [1. HR Management](#1-hr-management-hr)
  - [2. Payroll Management](#2-payroll-management-payroll)
  - [3. System Configuration](#3-system-configuration-settings)
  - [4. Authentication & Security](#4-authentication--security-auth)
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
- **Resilient IAM Session Management**: Automatic 401 token refresh retries with single-use OAuth2 refresh token rotation.
- **Zero Hardcoded Fake Values**: Animated `<Skeleton />` loaders during data fetch and clean, clear empty states.
- **Toast Notifications**: Built-in notifications using shadcn/ui Sonner (`toast.success`, `toast.error`, `toast.warning`).

---

## 📦 System Modules & Features

### 1. HR Management (`/hr`)

| Sub-Module | Route | Features & Capabilities |
| :--- | :--- | :--- |
| **Employee Directory** | `/hr/employees` | Grid & List views with smooth view toggles, multi-parameter search/filters (Department, Designation, Status), dynamic statistics cards, and printable Employee Summary Sheet. |
| **Employee Registration** | `/hr/employees/new` | Multi-step onboarding form capturing personal details, job roles, emergency contacts, bank details, and document uploads. |
| **Employee Profile** | `/hr/employees/[id]` | Comprehensive employee dossier displaying employment history, emergency contacts, job information, and edit dialogs. |
| **Attendance & Leave** | `/hr/attendance` | Attendance logs and leave requisitions tabs, interactive date range popover, metric summary cards (Present, Absent, Late, On Leave), and printable Attendance & Leave statements. |
| **Movement Tracking** | `/hr/movement` | Official and outside-factory movement logs with time pickers, status transitions (`Outside`, `Completed`), and Add/Edit modals. |
| **Holiday Set-up** | `/hr/holiday` | Calendar & List views for institutional holidays, department-specific or company-wide holiday assignment, and yearly summary statistics. |

---

### 2. Payroll Management (`/payroll`)

| Sub-Module | Route | Features & Capabilities |
| :--- | :--- | :--- |
| **Payroll Periods** | `/payroll/period` | Monthly/annual payroll cycle management, lock/unlock cycle actions, detailed salary sheet generation, and printable Annual Payroll Summary. |
| **Salary Import** | `/payroll/import` | Bulk attendance and overtime CSV/Excel data import with validation diagnostics and batch parsing. |
| **Salary Calculation** | `/payroll/calculation` | Automated gross salary, deductions (late, absent, tax, provident fund), allowances, overtime, and net pay computation. |
| **Advance Salary** | `/payroll/advance` | Employee salary advance requests, approval workflows, repayment installment schedules, and printable statements. |
| **Payslips** | `/payroll/payslip` | Employee payslip generator with interactive metric cards (featuring dynamic terracotta hover animations and facet watermarks), and 1-click printable Payslip Invoice modals. |

---

### 3. System Configuration (`/settings`)

| Sub-Module | Route | Features & Capabilities |
| :--- | :--- | :--- |
| **User Management** | `/settings/user-management` | **3-in-1 Unified Administration Suite**:<br>• **List of User**: User accounts, avatars, designations, roles, created details, and status.<br>• **Role Management**: Role definitions with assigned user counts and status.<br>• **Permission Management**: Granular matrix across all ERP modules (`View`, `Create`, `Edit`, `Delete`, `Export`, `Approve`, `All`).<br>• `+ Create User` and `+ Create Role` dialogs. |
| **General Setup** | `/settings/general-setup` | **9-Tab Enterprise Configuration Engine**:<br>• Designation • Department • Shift • Bank • Branch • Country • District • Division • Thana.<br>Includes full CRUD modals, search, and data tables. |
| **Production Setup** | `/settings/production-setup` | Assembly lines, machine configurations, and production parameters. |

---

### 4. Authentication & Security (`/auth`)

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
│   │   ├── hr/                       # HR routes (employees, attendance, movement, holiday)
│   │   ├── payroll/                  # Payroll routes (period, import, calculation, advance, payslip)
│   │   ├── settings/                 # System config routes (user-management, general-setup, production-setup)
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
│   ├── hr/                           # HR sub-features (employees, attendance, movement, holiday)
│   ├── payroll/                      # Payroll sub-features (period, import, calculation, advance, payslip)
│   └── settings/                     # Settings sub-features (user-management, general-setup)
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
