# PRISM - Post-Production Management System

## Project Documentation

---

## 1. Project Overview

### Purpose
PRISM (Post-Production Information & Resource Scheduling Management) is a comprehensive studio management software designed for post-production houses managing film, TV serial, web series, and advertisement editing workflows. The system provides end-to-end management of studio resources, bookings, billing, and conflict resolution.

### Business Use Case
- **Target Users**: Post-production studios, editing houses, dubbing studios, VFX facilities
- **Core Problem Solved**: Efficiently manage multiple editing rooms, editors, customer projects, and billing while avoiding scheduling conflicts
- **Key Benefits**:
  - Centralized booking management with conflict detection
  - Real-time visibility of room and editor availability
  - Automated chalan/invoice generation
  - Multi-company support for franchise operations

---

## 2. Tech Stack

### Frontend
- **Framework**: React 18 with TypeScript
- **UI Library**: Shadcn/ui (Radix UI primitives)
- **Styling**: Tailwind CSS
- **Routing**: Wouter
- **State Management**: TanStack Query (React Query)
- **Forms**: React Hook Form with Zod validation
- **Icons**: Lucide React

### Backend
- **Runtime**: Node.js
- **Framework**: Express.js
- **Language**: TypeScript
- **Build Tool**: TSX for development, ESBuild for production

### Database
- **Database**: PostgreSQL (Neon-backed)
- **ORM**: Drizzle ORM
- **Schema Validation**: Drizzle-Zod

### Authentication
- **Method**: Session-based authentication
- **Session Store**: Express Session with PostgreSQL (connect-pg-simple)
- **Security**: Security PIN verification per user/company combination

---

## 3. Project Modules

### 3.1 Master Module
Manages core reference data for the system.

| Sub-Module | Description |
|------------|-------------|
| Customers | Client companies with multiple contacts |
| Projects | Film/serial/ad projects linked to customers |
| Rooms | Editing rooms, sound studios, VFX suites |
| Editors | Staff editors with leave management |
| Designations | Contact designation master |

### 3.2 Operations Module
Day-to-day operational activities.

| Sub-Module | Description |
|------------|-------------|
| Bookings | Room scheduling with editor assignment |
| Calendar View | Visual day/week view of all bookings |
| Leaves | Editor leave management |

### 3.3 Reports Module
Analytics and reporting capabilities.

| Sub-Module | Description |
|------------|-------------|
| Booking Report | Filtered booking history with export |
| Chalan Report | Billing document history |
| Conflict Report | Scheduling conflicts analysis |
| Editor Report | Editor utilization metrics |

### 3.4 Utility Module
System administration features.

| Sub-Module | Description |
|------------|-------------|
| Users | User account management |
| User Rights | Role-based access control |

### 3.5 Chalan/Invoice
Billing documentation.

| Feature | Description |
|---------|-------------|
| Chalan Creation | Generate billing documents from bookings |
| Chalan Revision | Track changes with full audit trail |
| Chalan Cancellation | Cancel with reason tracking |

### 3.6 Conflict Report
Specialized conflict detection.

| Feature | Description |
|---------|-------------|
| Room Conflicts | Same room booked for overlapping times |
| Editor Conflicts | Same editor assigned to overlapping bookings |
| Leave Conflicts | Booking assigned to editor on leave |

### 3.7 User Management
Complete user lifecycle management.

| Feature | Description |
|---------|-------------|
| User Creation | Add new users with company assignment |
| Role Assignment | Admin, GST, Non-GST roles |
| Module Access | Granular permission control |
| Profile Management | User profile and PIN change |

---

## 4. Module Descriptions

### Master Module
**What it does**: Maintains all reference data that other modules depend on. Customers represent client companies, each with multiple contact persons. Projects belong to customers and represent specific work items. Rooms are physical or virtual spaces for editing. Editors are staff members who can be assigned to bookings.

**Who uses it**: Admin users primarily manage master data. Other users view this data when creating bookings.

### Operations Module
**What it does**: Core booking functionality allowing users to schedule rooms for specific projects with optional editor assignment. Supports repeat bookings, status management (planning, tentative, confirmed, cancelled), and actual time tracking.

**Who uses it**: All authenticated users based on their permissions.

### Reports Module
**What it does**: Provides filtered views and analytics on system data. Booking reports show historical booking data. Chalan reports track billing documents. Conflict reports highlight scheduling issues. Editor reports show workload distribution.

**Who uses it**: Admin and management users for operational oversight.

### Utility Module
**What it does**: System administration including user account management and granular permission control per module/section.

**Who uses it**: Admin users only.

### Chalan/Invoice
**What it does**: Generates billing documents (chalans) from confirmed bookings. Tracks line items, revisions, and cancellations with full audit trail.

**Who uses it**: Billing/accounting staff and admin users.

---

## 5. Workflow Descriptions

### 5.1 Login -> Company Selection Workflow
1. User navigates to login page
2. System displays company dropdown (fetched from /api/companies)
3. User enters username, security PIN, and selects company
4. System validates credentials against user record for that company
5. On success, session is created with userId, userRole, and companyId
6. User is redirected to dashboard with company context applied

### 5.2 Booking Flow
1. User navigates to Booking module
2. Selects date, room, and time slot
3. Selects customer, then project (filtered by customer)
4. Optionally assigns editor
5. System checks for conflicts (room/editor overlap, editor on leave)
6. If no conflicts, booking is created with "planning" status
7. User can update status to "tentative" or "confirmed"
8. All changes are logged in booking_logs table

### 5.3 Conflict Flow
1. User requests conflict report for date range
2. System analyzes all bookings in range
3. Identifies room conflicts (same room, overlapping times)
4. Identifies editor conflicts (same editor, overlapping times)
5. Flags bookings where editor is on approved leave
6. Results displayed with affected bookings and conflict details

### 5.4 Chalan Creation Flow
1. User navigates to Chalan module
2. Selects confirmed booking or creates new chalan
3. Adds customer, project, and line items
4. System generates unique chalan number (format: PRISM-YYYYMMDD-XXX)
5. Chalan is saved with all items
6. Any updates create revision records for audit trail

### 5.5 Report Generation Flow
1. User selects report type (Booking, Chalan, Conflict, Editor)
2. Applies filters (date range, room, customer, editor)
3. System queries database with filters
4. Results displayed in data table format
5. Export options available for external use

---

## 6. Role & Permission Logic

### User Roles
| Role | Description | Default Permissions |
|------|-------------|-------------------|
| Admin | Full system access | All modules, all actions |
| GST | Standard user with GST capabilities | Most modules, limited admin |
| Non-GST | Basic user access | View and basic operations |

### Permission Structure
Each module/section has four permission types:
- **canView**: Read access to module data
- **canCreate**: Create new records
- **canEdit**: Modify existing records
- **canDelete**: Remove records

### Permission Enforcement
1. Backend routes check user role from session
2. Admin endpoints (users, access) require "admin" role
3. Other endpoints check specific module permissions via userModuleAccess table
4. Frontend hides/disables UI elements based on permissions

---

## 7. Data Flow Diagram (Text Explanation)

```
[User Login]
     |
     v
[Session Created] --> [Company Context Set]
     |
     v
[Dashboard] --> Shows summary based on company
     |
     +---> [Masters] --> CRUD operations
     |         |
     |         +--> [Customers] --> [Contacts]
     |         +--> [Projects] --> linked to Customers
     |         +--> [Rooms]
     |         +--> [Editors] --> [Leaves]
     |
     +---> [Operations]
     |         |
     |         +--> [Bookings] --> [Conflict Check]
     |                   |                |
     |                   |                +--> [Room Availability]
     |                   |                +--> [Editor Availability]
     |                   |                +--> [Leave Check]
     |                   |
     |                   +--> [Booking Logs] (Audit)
     |
     +---> [Chalan]
     |         |
     |         +--> [Create from Booking]
     |         +--> [Line Items]
     |         +--> [Revisions] (Audit)
     |
     +---> [Reports]
               |
               +--> Aggregate data from Bookings, Chalans, Editors
```

---

## 8. Database Schema Summary

### Core Tables
| Table | Description |
|-------|-------------|
| companies | Multi-company support |
| users | User accounts with company assignment |
| customers | Client companies |
| customer_contacts | Multiple contacts per customer |
| designations | Contact designation master |
| projects | Work items linked to customers |
| rooms | Physical/virtual editing spaces |
| editors | Staff editors |

### Operational Tables
| Table | Description |
|-------|-------------|
| bookings | Room reservations |
| booking_logs | Audit trail for bookings |
| editor_leaves | Leave records |

### Billing Tables
| Table | Description |
|-------|-------------|
| chalans | Billing documents |
| chalan_items | Line items for chalans |
| chalan_revisions | Change history |

### Access Control
| Table | Description |
|-------|-------------|
| user_module_access | Granular permissions |

---

## 9. API Endpoints Summary

### Authentication
- `POST /api/auth/login` - User login
- `POST /api/auth/logout` - User logout
- `GET /api/auth/session` - Get current session

### Companies
- `GET /api/companies` - List companies (public)
- `POST /api/companies` - Create company

### Masters
- `GET/POST/PATCH/DELETE /api/customers` - Customer CRUD
- `GET/POST/PATCH/DELETE /api/projects` - Project CRUD
- `GET/POST/PATCH/DELETE /api/rooms` - Room CRUD
- `GET/POST/PATCH/DELETE /api/editors` - Editor CRUD

### Operations
- `GET/POST/PATCH /api/bookings` - Booking operations
- `POST /api/bookings/:id/cancel` - Cancel booking
- `POST /api/bookings/check-conflicts` - Conflict detection
- `GET/POST/PATCH/DELETE /api/editor-leaves` - Leave management

### Billing
- `GET/POST /api/chalans` - Chalan operations
- `POST /api/chalans/:id/cancel` - Cancel chalan
- `GET /api/chalans/:id/revisions` - Revision history

### Reports
- `GET /api/reports/conflicts` - Conflict report
- `GET /api/reports/editor` - Editor utilization

---

## 10. Future Enhancement Suggestions

### Short Term
1. **Dashboard Analytics**: Add visual charts for booking trends, room utilization
2. **Email Notifications**: Booking confirmations, conflict alerts
3. **PDF Export**: Generate professional PDF chalans/invoices
4. **Bulk Operations**: Multi-select for status updates

### Medium Term
1. **Calendar Integration**: Google Calendar / Outlook sync
2. **Mobile App**: React Native companion app
3. **Client Portal**: Customer-facing booking requests
4. **Equipment Tracking**: Add equipment/asset management

### Long Term
1. **Multi-Location Support**: Geographic location management
2. **Financial Reports**: Revenue, outstanding, collections
3. **API Integration**: Third-party accounting software
4. **Workflow Automation**: Automated status transitions

---

## 11. Installation & Setup

### Prerequisites
- Node.js 20+
- PostgreSQL database
- npm or yarn package manager

### Environment Variables
```
DATABASE_URL=postgresql://...
PGHOST=...
PGPORT=5432
PGUSER=...
PGPASSWORD=...
PGDATABASE=...
```

### Commands
```bash
# Install dependencies
npm install

# Push database schema
npm run db:push

# Start development server
npm run dev

# Build for production
npm run build

# Start production server
npm run start
```

---

## Document Information
- **Version**: 1.0
- **Last Updated**: December 2024
- **Author**: PRISM Development Team
