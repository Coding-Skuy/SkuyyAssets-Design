# Product Requirements Document (PRD)

## SkuyAssets Pro

**Version:** 1.0
**Status:** Draft
**Product Manager:** [Product Manager Name]
**Engineering Lead:** [Engineering Lead Name]
**Design Lead:** [Design Lead Name]
**Last Updated:** July 2, 2026

---

# 1. Pengantar

## 1.1 Tujuan Dokumen

Dokumen PRD ini menerjemahkan Business Requirements Document (BRD) SkuyAssets Pro ke dalam spesifikasi produk yang detail dan actionable untuk tim engineering, design, dan QA. Dokumen ini mencakup:

- Spesifikasi fitur yang detail dengan acceptance criteria
- Data models dan technical architecture
- User stories dan user flows
- Non-functional requirements
- Success metrics dan KPIs

## 1.2 Ringkasan Produk

**SkuyAssets Pro** adalah platform manajemen aset perusahaan berbasis mobile yang dirancang untuk membantu organisasi mengelola seluruh siklus hidup aset secara digital, efisien, dan berbasis data.

### Key Differentiators:
- **AI-First Approach**: Mengintegrasikan AI untuk query, insights, dan report generation
- **Mobile-First**: Dirancang untuk penggunaan lapangan dengan QR/Barcode scanning
- **Serverless Architecture**: Low operational cost, scalable, easy maintenance
- **Real-time Dashboard**: Monitoring kondisi aset secara real-time

### Tech Stack:
- **Frontend**: Flutter (iOS & Android)
- **Backend**: Firebase (Firestore, Authentication, Storage, Cloud Functions)
- **AI**: Firebase Genkit + Google Gemini
- **Analytics**: Firebase Analytics
- **Monitoring**: Firebase Crashlytics

---

# 2. Tujuan & Objektif Produk

## 2.1 Business Goals

### Goal 1: Digitalisasi Proses Manajemen Aset
**Objektif Terukur:**
- 100% aset perusahaan tercatat di sistem dalam 3 bulan sejak deployment
- Eliminasi penggunaan spreadsheet untuk tracking aset
- Seluruh transaksi aset (borrow, transfer, maintenance) tercatat digital

### Goal 2: Mengurangi Waktu Audit Aset >50%
**Objektif Terukur:**
- Baseline audit manual: ~2 minggu untuk 1000 aset
- Target audit dengan app: <1 minggu untuk 1000 aset
- QR scan time: <5 detik per aset
- Audit report generation: <2 menit

### Goal 3: Mengurangi Risiko Kehilangan Aset
**Objektif Terukur:**
- Real-time location tracking untuk semua aset
- Alert untuk aset yang tidak ter-scan >90 hari
- 100% aset memiliki responsible person
- Audit trail lengkap untuk setiap perubahan

### Goal 4: Dashboard Aset Real-time
**Objektif Terukur:**
- Dashboard load time <2 detik
- Data synchronization <5 detik setelah perubahan
- 10+ metrics visualized (total aset, status, location, maintenance, dll)

### Goal 5: Maintenance Proaktif dengan AI
**Objektif Terukur:**
- AI prediction untuk maintenance timing (berdasarkan usage pattern)
- Alert otomatis untuk aset yang mendekati maintenance schedule
- Maintenance cost tracking dan optimization suggestions

### Goal 6: Laporan Otomatis
**Objektif Terukur:**
- Report generation via natural language prompt
- Export format: PDF, Excel, CSV
- Scheduled reports (daily, weekly, monthly)
- <30 detik untuk generate laporan bulanan

---

# 3. User Roles & Permissions

## 3.1 Role Definitions

### Super Admin
**Deskripsi:** Owner atau top-level administrator perusahaan

**Permissions:**
- ✅ Full access ke semua fitur
- ✅ Company settings management
- ✅ User management (create, edit, delete, assign roles)
- ✅ Department management
- ✅ View all assets across departments
- ✅ Access all reports
- ✅ System configuration
- ✅ Billing & subscription management

---

### Asset Administrator
**Deskripsi:** Mengelola master data aset perusahaan

**Permissions:**
- ✅ CRUD assets (all departments)
- ✅ CRUD asset categories
- ✅ Generate QR codes
- ✅ Import/export asset data
- ✅ View all transactions
- ✅ Generate reports
- ✅ Dashboard access (company-wide)
- ✅ Manage asset transfers
- ❌ User management
- ❌ System settings

---

### Asset Officer
**Deskripsi:** Melakukan inventarisasi, audit, dan pemeliharaan aset

**Permissions:**
- ✅ CRUD assets (own department only)
- ✅ Scan QR/Barcode
- ✅ Create borrow/return transactions
- ✅ Log maintenance records
- ✅ Update asset status & location
- ✅ View asset history
- ✅ Dashboard access (department-level)
- ✅ Generate department reports
- ❌ Delete assets
- ❌ User management

---

### Warehouse Staff
**Deskripsi:** Mengelola aset di gudang

**Permissions:**
- ✅ View assets (warehouse location only)
- ✅ Scan QR/Barcode
- ✅ Update asset location (within warehouse)
- ✅ Process borrow/return for warehouse assets
- ✅ Basic asset info update
- ❌ Create/delete assets
- ❌ Financial data access
- ❌ Reports

---

### IT Support
**Deskripsi:** Mengelola aset teknologi (laptop, server, printer, dll)

**Permissions:**
- ✅ CRUD tech assets (category: IT Equipment)
- ✅ Scan QR/Barcode
- ✅ Schedule maintenance
- ✅ Log maintenance records
- ✅ View tech asset reports
- ✅ Dashboard access (IT assets only)
- ❌ Non-IT assets management
- ❌ User management

---

### Finance
**Deskripsi:** Melihat nilai dan laporan keuangan aset

**Permissions:**
- ✅ View all assets (read-only)
- ✅ View purchase price & depreciation
- ✅ Generate financial reports
- ✅ Export data
- ✅ Dashboard access (financial metrics)
- ❌ Edit assets
- ❌ Transactions

---

### Manager
**Deskripsi:** Operational/Branch Manager yang memonitor aset

**Permissions:**
- ✅ View assets (own department/branch)
- ✅ Approve borrow requests
- ✅ Dashboard access (department-level)
- ✅ Generate reports
- ✅ AI Assistant access
- ❌ Edit assets
- ❌ User management

---

### Staff (Regular User)
**Deskripsi:** Karyawan yang meminjam aset

**Permissions:**
- ✅ View available assets
- ✅ Create borrow request
- ✅ Return borrowed assets
- ✅ View own borrow history
- ❌ All other features

---

## 3.2 Permission Matrix

| Feature | Super Admin | Asset Admin | Asset Officer | Warehouse | IT Support | Finance | Manager | Staff |
|---------|-------------|-------------|---------------|-----------|------------|---------|---------|-------|
| User Management | ✅ | ❌ | ❌ | ❌ | ❌ | ❌ | ❌ | ❌ |
| Company Settings | ✅ | ❌ | ❌ | ❌ | ❌ | ❌ | ❌ | ❌ |
| Create Asset | ✅ | ✅ | ✅ | ❌ | ✅* | ❌ | ❌ | ❌ |
| Edit Asset | ✅ | ✅ | ✅ | ✅** | ✅* | ❌ | ❌ | ❌ |
| Delete Asset | ✅ | ✅ | ❌ | ❌ | ❌ | ❌ | ❌ | ❌ |
| Scan QR/Barcode | ✅ | ✅ | ✅ | ✅ | ✅ | ❌ | ❌ | ❌ |
| Borrow Asset | ✅ | ✅ | ✅ | ✅ | ✅ | ❌ | ❌ | ✅ |
| Approve Borrow | ✅ | ✅ | ❌ | ❌ | ❌ | ❌ | ✅ | ❌ |
| Maintenance Log | ✅ | ✅ | ✅ | ❌ | ✅ | ❌ | ❌ | ❌ |
| View All Assets | ✅ | ✅ | ❌*** | ❌*** | ❌*** | ✅ | ❌*** | ❌ |
| Dashboard | ✅ | ✅ | ✅*** | ❌*** | ✅*** | ✅ | ✅*** | ❌ |
| Reports | ✅ | ✅ | ✅*** | ❌ | ✅*** | ✅ | ✅*** | ❌ |
| AI Assistant | ✅ | ✅ | ✅ | ❌ | ✅ | ✅ | ✅ | ❌ |

*IT Support hanya untuk aset kategori IT
**Warehouse Staff hanya update lokasi
***Scope terbatas (department/category)

---

# 4. Detail Fitur

## 4.1 Authentication & Authorization

### Priority: P0 (Must Have)

### Deskripsi
Sistem autentikasi dan otorisasi menggunakan Firebase Authentication dengan role-based access control (RBAC).

### User Stories

**US-AUTH-001: Login dengan Email/Password**
```
Sebagai user,
Saya ingin login menggunakan email dan password,
Supaya saya bisa mengakses aplikasi dengan aman.
```

**US-AUTH-002: Multi-role Support**
```
Sebagai Super Admin,
Saya ingin mengatur role untuk setiap user,
Supaya akses ke fitur dapat dikontrol sesuai tanggung jawab.
```

**US-AUTH-003: Password Reset**
```
Sebagai user yang lupa password,
Saya ingin menerima email reset password,
Supaya saya bisa membuat password baru.
```

### Acceptance Criteria

**AC-AUTH-001:**
- ✅ User dapat login dengan email & password yang valid
- ✅ Error message jelas untuk invalid credentials
- ✅ Session persists setelah app closed
- ✅ Auto-logout setelah 30 hari inactivity
- ✅ Login attempt limited (5 attempts dalam 15 menit)

**AC-AUTH-002:**
- ✅ Firebase Auth UID tersimpan di Firestore users collection
- ✅ Role assignment tersimpan di user document
- ✅ Role validation di client dan server side (Security Rules)
- ✅ Permission check sebelum akses setiap fitur

**AC-AUTH-003:**
- ✅ "Forgot Password" button tersedia di login screen
- ✅ Email reset password terkirim dalam <30 detik
- ✅ Reset link valid selama 24 jam
- ✅ Konfirmasi setelah password berhasil direset

### Technical Notes
- Firebase Authentication Email/Password provider
- Custom claims untuk role storage (untuk Firebase Security Rules)
- Cloud Function untuk assign custom claims setelah user creation
- Firestore Security Rules validate role untuk setiap operation

### Dependencies
- Firebase Authentication setup
- Cloud Functions deployment

---

## 4.2 Company & Department Management

### Priority: P0 (Must Have)

### Deskripsi
Pengelolaan struktur organisasi (company dan departments). MVP: Single company per deployment, multi-department support.

### User Stories

**US-COMP-001: Setup Company Profile**
```
Sebagai Super Admin,
Saya ingin mengatur profil perusahaan (nama, logo, alamat),
Supaya informasi perusahaan tersimpan di sistem.
```

**US-DEPT-001: Manage Departments**
```
Sebagai Super Admin,
Saya ingin create/edit/delete department,
Supaya struktur organisasi tercatat di sistem.
```

**US-DEPT-002: Assign Users to Department**
```
Sebagai Super Admin,
Saya ingin assign user ke department tertentu,
Supaya user hanya bisa akses aset di department-nya.
```

### Acceptance Criteria

**AC-COMP-001:**
- ✅ Form input: company name (required), logo (optional), address, phone, email
- ✅ Logo upload max 2MB, format: jpg/png
- ✅ Data tersimpan di Firestore `/companies/{companyId}`
- ✅ Company profile tampil di app header

**AC-DEPT-001:**
- ✅ Department list view dengan search & filter
- ✅ Create department form: name (required), code (unique), description
- ✅ Edit department (hanya jika tidak ada aset aktif atau dengan konfirmasi)
- ✅ Delete department (soft delete, hanya jika tidak ada aset aktif)
- ✅ Department hierarchy support (optional: parent department)

**AC-DEPT-002:**
- ✅ User form memiliki dropdown department selection
- ✅ User hanya bisa di-assign ke 1 department
- ✅ Perubahan department tercatat di audit log

### Technical Notes
- Single company mode: `companyId` hardcoded atau dari config
- Departments stored di `/departments` collection
- Users reference departmentId
- Firestore index untuk department queries

---

## 4.3 Asset Management (CRUD)

### Priority: P0 (Must Have)

### Deskripsi
Core functionality untuk create, read, update, delete aset. Setiap aset memiliki QR code unik, kategori, status, lokasi, dan responsible person.

### User Stories

**US-ASSET-001: Create Asset**
```
Sebagai Asset Administrator,
Saya ingin menambahkan aset baru ke sistem,
Supaya aset tercatat dan bisa dikelola.
```

**US-ASSET-002: View Asset List**
```
Sebagai Asset Officer,
Saya ingin melihat list aset dengan filter dan search,
Supaya saya bisa menemukan aset dengan cepat.
```

**US-ASSET-003: View Asset Detail**
```
Sebagai user dengan akses view,
Saya ingin melihat detail lengkap aset termasuk histori,
Supaya saya memahami kondisi dan riwayat aset.
```

**US-ASSET-004: Edit Asset**
```
Sebagai Asset Officer,
Saya ingin mengupdate informasi aset,
Supaya data aset selalu akurat.
```

**US-ASSET-005: Delete Asset**
```
Sebagai Asset Administrator,
Saya ingin menghapus aset yang sudah disposal,
Supaya data tetap clean dan akurat.
```

### Acceptance Criteria

**AC-ASSET-001: Create Asset Form**
- ✅ Required fields: Asset Name, Category, Department, Status
- ✅ Optional fields: Brand, Model, Serial Number, Purchase Date, Purchase Price, Warranty Until, Supplier, Location, Description
- ✅ Auto-generate Asset Code (format: CAT-DEPT-YYYYMMDD-XXX, contoh: IT-HRD-20260702-001)
- ✅ Auto-generate QR Code setelah asset created
- ✅ Upload multiple images (max 5 images, max 5MB per image)
- ✅ Assign responsible person (user dropdown)
- ✅ Validation: serial number unique per category
- ✅ Success message + redirect ke asset detail

**AC-ASSET-002: Asset List View**
- ✅ Table/card view dengan columns: Thumbnail, Asset Code, Name, Category, Status, Location, Responsible Person
- ✅ Search by: asset code, name, serial number
- ✅ Filter by: category, status, department, location, responsible person
- ✅ Sort by: name, purchase date, last updated
- ✅ Pagination (20 items per page)
- ✅ Pull-to-refresh
- ✅ Bulk actions: export selected, bulk QR print

**AC-ASSET-003: Asset Detail View**
- ✅ Display all asset information
- ✅ Image gallery (swipe untuk multiple images)
- ✅ QR code display (tap untuk enlarge, long-press untuk download)
- ✅ Tabs: Overview, History, Maintenance, Documents
- ✅ Action buttons: Edit, Scan QR, Transfer, Borrow, Log Maintenance, Delete (based on role)
- ✅ Timeline view untuk asset history (created, transferred, borrowed, returned, maintained)

**AC-ASSET-004: Edit Asset**
- ✅ Form pre-filled dengan data existing
- ✅ All fields editable except: Asset Code, QR Code
- ✅ Changes logged di audit trail
- ✅ Validation sama seperti create
- ✅ Konfirmasi sebelum save changes
- ✅ Real-time sync ke semua devices

**AC-ASSET-005: Delete Asset**
- ✅ Soft delete (status changed to "DELETED", data retained)
- ✅ Confirmation dialog dengan warning
- ✅ Only Super Admin dan Asset Administrator can delete
- ✅ Cannot delete if asset sedang dipinjam (status = "BORROWED")
- ✅ Deleted assets hidden dari list (toggle untuk show deleted)
- ✅ Restore option untuk deleted assets (dalam 30 hari)

### Technical Notes
- Firestore collection: `/assets/{assetId}`
- Asset images stored di Firebase Storage: `/assets/{assetId}/images/{imageId}.jpg`
- QR code generated via Cloud Function, stored as data URL di Firestore
- Search implemented dengan Firestore composite indexes + Algolia (V2)
- Real-time updates menggunakan Firestore snapshot listeners

### Dependencies
- Category Management (Section 4.4)
- QR Code Generation (Section 4.5)
- Firebase Storage setup
- Cloud Functions untuk QR generation

---

## 4.4 Asset Categories & Status

### Priority: P0 (Must Have)

### Deskripsi
Master data untuk kategori aset dan status lifecycle aset.

### User Stories

**US-CAT-001: Manage Asset Categories**
```
Sebagai Asset Administrator,
Saya ingin mengelola kategori aset,
Supaya aset dapat diklasifikasikan dengan benar.
```

**US-STATUS-001: Asset Status Tracking**
```
Sebagai sistem,
Saya ingin otomatis update status aset berdasarkan transaksi,
Supaya status aset selalu akurat.
```

### Acceptance Criteria

**AC-CAT-001: Category Management**
- ✅ Default categories: IT Equipment, Furniture, Vehicle, Building, Office Supplies, Other
- ✅ CRUD categories (Super Admin & Asset Administrator only)
- ✅ Category fields: Name (required), Code (unique), Icon, Description
- ✅ Cannot delete category jika ada aset aktif
- ✅ Category dropdown di asset form

**AC-STATUS-001: Asset Status Lifecycle**
- ✅ Predefined statuses:
  - **AVAILABLE**: Aset tersedia untuk dipinjam/digunakan
  - **IN_USE**: Aset sedang digunakan (assigned to person permanently)
  - **BORROWED**: Aset sedang dipinjam
  - **MAINTENANCE**: Aset dalam perbaikan/servis
  - **RETIRED**: Aset sudah tidak digunakan tapi masih ada
  - **DELETED**: Soft deleted
- ✅ Status color coding (green, blue, orange, red, gray, black)
- ✅ Status auto-update rules:
  - AVAILABLE → BORROWED (saat borrow transaction created)
  - BORROWED → AVAILABLE (saat return transaction completed)
  - Any → MAINTENANCE (saat maintenance log created dengan status "In Progress")
  - MAINTENANCE → AVAILABLE (saat maintenance completed)
- ✅ Status change logged di audit trail
- ✅ Manual status override (dengan reason) oleh Admin

### Technical Notes
- Categories: `/categories` collection
- Status: enum di asset document, tidak perlu separate collection
- Status history tracked di `/assets/{assetId}/statusHistory` subcollection
- Cloud Function triggers untuk auto status update

---

## 4.5 QR Code Generation & Barcode Scanner

### Priority: P0 (Must Have)

### Deskripsi
Generate unique QR code untuk setiap aset dan scan QR/barcode untuk quick access.

### User Stories

**US-QR-001: Auto QR Generation**
```
Sebagai sistem,
Saya ingin otomatis generate QR code saat aset dibuat,
Supaya setiap aset memiliki identifier unik.
```

**US-QR-002: Scan QR Code**
```
Sebagai Asset Officer,
Saya ingin scan QR code untuk langsung buka detail aset,
Supaya inventarisasi lebih cepat.
```

**US-QR-003: Batch QR Print**
```
Sebagai Asset Administrator,
Saya ingin download QR code multiple asets untuk print,
Supaya bisa cetak sticker QR secara bulk.
```

**US-QR-004: Scan External Barcode**
```
Sebagai Asset Officer,
Saya ingin scan barcode yang sudah ada di aset,
Supaya bisa cari aset by barcode.
```

### Acceptance Criteria

**AC-QR-001: QR Code Generation**
- ✅ QR code auto-generated saat asset created via Cloud Function
- ✅ QR content: Deep link format `skuyassets://asset/{assetId}`
- ✅ QR code stored as base64 data URL di Firestore
- ✅ QR design: Logo SkuyAssets di center, asset code di bottom
- ✅ Generation time: <3 seconds
- ✅ Re-generate option jika QR rusak/hilang

**AC-QR-002: QR Scanner**
- ✅ Camera permission request dengan explanation
- ✅ QR scanner UI dengan focus box dan flash toggle
- ✅ Scan detection time: <2 seconds
- ✅ Success feedback: vibration + sound + visual confirmation
- ✅ Auto-redirect ke asset detail setelah scan
- ✅ Error handling: invalid QR, network error, asset not found
- ✅ Scan history (last 10 scanned assets) untuk quick access

**AC-QR-003: Batch QR Export**
- ✅ Select multiple assets dari list
- ✅ Generate PDF dengan QR stickers layout (4x6 per page, A4 size)
- ✅ Each sticker: QR code + Asset Name + Asset Code
- ✅ Download PDF (<10 seconds untuk 100 QR codes)
- ✅ Option: sticker size (small/medium/large)

**AC-QR-004: Barcode Scanner**
- ✅ Support barcode formats: EAN-13, CODE-128, UPC-A, CODE-39
- ✅ Search asset by barcode setelah scan
- ✅ Jika asset found: redirect ke detail
- ✅ Jika asset not found: option untuk create asset dengan barcode ter-pre-filled di serial number
- ✅ Same scanner UI untuk QR dan Barcode (auto-detect)

### Technical Notes
- QR generation: Cloud Function menggunakan library `qrcode` atau `node-qrcode`
- Scanner: Flutter package `mobile_scanner` atau `qr_code_scanner`
- Deep linking: Flutter `uni_links` package
- PDF generation: `pdf` package untuk Flutter
- Barcode formats: `mobile_scanner` supports multiple formats

### Dependencies
- Firebase Cloud Functions
- Flutter camera permission
- Deep link configuration (Android & iOS)

---

## 4.6 Asset Transfer

### Priority: P0 (Must Have)

### Deskripsi
Transfer kepemilikan aset antar department atau antar responsible person.

### User Stories

**US-TRANSFER-001: Transfer Asset**
```
Sebagai Asset Officer,
Saya ingin transfer aset ke department/person lain,
Supaya kepemilikan aset tercatat dengan benar.
```

**US-TRANSFER-002: Transfer History**
```
Sebagai Manager,
Saya ingin melihat histori transfer aset,
Supaya saya bisa audit pergerakan aset.
```

### Acceptance Criteria

**AC-TRANSFER-001: Transfer Form**
- ✅ Input fields: From Department/Person (auto-filled), To Department (required), To Person (required), Transfer Date, Reason/Notes
- ✅ Asset status validation: cannot transfer if status = BORROWED or MAINTENANCE
- ✅ Confirmation dialog dengan summary transfer
- ✅ Success notification ke previous dan new responsible person
- ✅ Transfer record saved dengan timestamp dan transferrer info
- ✅ Asset detail updated: department, responsible person, location

**AC-TRANSFER-002: Transfer History**
- ✅ Timeline view di asset detail tab "History"
- ✅ Each entry shows: Date, From → To, Transferrer, Reason
- ✅ Filter transfer records by date range, department, person
- ✅ Export transfer report (PDF/Excel)

### Technical Notes
- Transfer records: subcollection `/assets/{assetId}/transfers/{transferId}`
- Cloud Function trigger untuk notification
- Update asset document fields: departmentId, responsiblePersonId, updatedAt

---

## 4.7 Borrow & Return System

### Priority: P0 (Must Have)

### Deskripsi
Sistem peminjaman aset dengan approval workflow sederhana.

### User Stories

**US-BORROW-001: Request Borrow Asset**
```
Sebagai Staff,
Saya ingin request pinjam aset,
Supaya saya bisa gunakan aset untuk pekerjaan.
```

**US-BORROW-002: Approve Borrow Request**
```
Sebagai Manager,
Saya ingin approve/reject borrow request,
Supaya aset terkontrol dan tidak disalahgunakan.
```

**US-BORROW-003: Return Asset**
```
Sebagai Staff yang meminjam,
Saya ingin return aset yang sudah selesai digunakan,
Supaya aset kembali tersedia.
```

**US-BORROW-004: Overdue Notification**
```
Sebagai sistem,
Saya ingin kirim notifikasi untuk aset yang overdue,
Supaya borrower aware dan segera return.
```

### Acceptance Criteria

**AC-BORROW-001: Borrow Request Form**
- ✅ Available dari asset detail (button "Borrow")
- ✅ Form fields: Borrow Date (default: today), Due Date (required), Purpose/Reason (required)
- ✅ Due date validation: must be future date, max 90 days
- ✅ Asset status check: hanya AVAILABLE assets bisa dipinjam
- ✅ Request status: PENDING (waiting approval)
- ✅ Notification sent to Manager untuk approval

**AC-BORROW-002: Approval Workflow**
- ✅ Manager receives push notification dengan asset info
- ✅ Approval page: view requester, asset detail, purpose, dates
- ✅ Actions: Approve or Reject (dengan reason jika reject)
- ✅ Approve: asset status → BORROWED, borrower = requester, notification sent to requester
- ✅ Reject: request status → REJECTED, notification sent dengan reason
- ✅ Auto-approve jika tidak ada approval dalam 24 jam (configurable)

**AC-BORROW-003: Return Process**
- ✅ "Return Asset" button di borrow transaction detail
- ✅ Return form: Return Date (default: today), Condition (dropdown: Good/Fair/Damaged), Notes
- ✅ Upload return photos (optional, max 3 photos)
- ✅ Jika condition = Damaged: auto-create maintenance record
- ✅ Transaction status → COMPLETED
- ✅ Asset status → AVAILABLE (atau MAINTENANCE jika damaged)
- ✅ Notification sent to Manager

**AC-BORROW-004: Overdue Management**
- ✅ Scheduled job (daily at 9 AM): check overdue borrowings
- ✅ Overdue = current date > due date && status = BORROWED
- ✅ Notification sent to borrower: "Your borrowed asset [Asset Name] is overdue. Please return immediately."
- ✅ CC notification to Manager
- ✅ Overdue indicator (red badge) di borrow list
- ✅ Escalation: if overdue >7 days, notify Super Admin

### Technical Notes
- Collection: `/borrowTransactions/{transactionId}`
- Fields: assetId, borrowerId, approverId, requestDate, approvalDate, borrowDate, dueDate, returnDate, status (PENDING/APPROVED/REJECTED/COMPLETED), condition, notes, photos[]
- Cloud Scheduler + Cloud Function untuk daily overdue check
- Push notifications via FCM

### Dependencies
- Notification system (Section 4.9)
- Cloud Scheduler
- Firebase Cloud Messaging

---

## 4.8 Maintenance History & Scheduling

### Priority: P0 (Must Have)

### Deskripsi
Pencatatan histori maintenance dan penjadwalan maintenance berkala.

### User Stories

**US-MAINT-001: Log Maintenance**
```
Sebagai Asset Officer,
Saya ingin log maintenance yang sudah dilakukan,
Supaya histori servis tercatat.
```

**US-MAINT-002: Schedule Maintenance**
```
Sebagai IT Support,
Saya ingin schedule maintenance berkala,
Supaya aset tidak telat dirawat.
```

**US-MAINT-003: Maintenance Reminder**
```
Sebagai sistem,
Saya ingin kirim reminder untuk maintenance yang akan datang,
Supaya maintenance dilakukan tepat waktu.
```

### Acceptance Criteria

**AC-MAINT-001: Maintenance Log Form**
- ✅ Available dari asset detail (button "Log Maintenance")
- ✅ Form fields: Type (dropdown: Preventive/Corrective/Emergency), Date (required), Vendor/Technician, Cost, Status (dropdown: Scheduled/In Progress/Completed), Description, Notes
- ✅ Upload photos/documents (optional, max 5 files)
- ✅ If status = "In Progress": asset status auto-update → MAINTENANCE
- ✅ If status = "Completed": asset status auto-update → AVAILABLE
- ✅ Maintenance record saved dengan timestamp

**AC-MAINT-002: Maintenance Schedule**
- ✅ "Schedule Maintenance" form: Type, Scheduled Date (future), Recurrence (None/Monthly/Quarterly/Yearly), Vendor, Estimated Cost, Description
- ✅ Recurring maintenance auto-create next schedule setelah completed
- ✅ Calendar view untuk scheduled maintenance
- ✅ Notification sent 7 days before scheduled date

**AC-MAINT-003: Maintenance History View**
- ✅ Timeline view di asset detail tab "Maintenance"
- ✅ Each entry: Date, Type, Vendor, Cost, Status, Description
- ✅ Total maintenance cost summary
- ✅ Filter by date range, type, status
- ✅ Export maintenance report per asset atau per date range

### Technical Notes
- Collection: `/maintenanceRecords/{recordId}`
- Fields: assetId, type, date, scheduledDate, recurrence, vendorName, cost, status, description, notes, photos[], createdBy, createdAt
- Cloud Scheduler untuk maintenance reminders
- Recurring logic: Cloud Function triggered on completion untuk create next schedule

---

## 4.9 Dashboard & Analytics

### Priority: P0 (Must Have)

### Deskripsi
Real-time dashboard untuk monitoring kondisi aset dengan berbagai metrics dan visualisasi.

### User Stories

**US-DASH-001: Company Overview Dashboard**
```
Sebagai Manager,
Saya ingin melihat overview kondisi semua aset,
Supaya saya bisa monitor kesehatan aset perusahaan.
```

**US-DASH-002: Department Dashboard**
```
Sebagai Asset Officer,
Saya ingin melihat dashboard aset di department saya,
Supaya saya fokus ke aset yang menjadi tanggung jawab saya.
```

### Acceptance Criteria

**AC-DASH-001: Key Metrics Cards**
- ✅ Total Assets (dengan trend vs bulan lalu)
- ✅ Assets by Status (pie chart: Available, In Use, Borrowed, Maintenance, Retired)
- ✅ Assets by Category (bar chart)
- ✅ Total Asset Value (sum purchase price)
- ✅ Active Borrowings (dengan overdue count)
- ✅ Maintenance This Month (count + total cost)
- ✅ Metrics auto-refresh setiap 30 seconds

**AC-DASH-002: Visualizations**
- ✅ Asset Status Distribution (donut chart)
- ✅ Asset Categories (horizontal bar chart)
- ✅ Assets by Location (list dengan count)
- ✅ Recent Activities Timeline (last 20 activities: created, borrowed, returned, maintained)
- ✅ Top 5 Most Borrowed Assets (bar chart)
- ✅ Maintenance Cost Trend (line chart, last 6 months)

**AC-DASH-003: Quick Actions**
- ✅ "Add Asset" FAB (Floating Action Button)
- ✅ "Scan QR" quick button
- ✅ "Overdue Borrowings" quick link (badge jika ada)
- ✅ "Pending Approvals" quick link (Manager only, badge jika ada)

**AC-DASH-004: Performance**
- ✅ Dashboard load time: <2 seconds
- ✅ Data refresh without full page reload
- ✅ Skeleton loading untuk better UX
- ✅ Cache strategy untuk data yang jarang berubah (categories, departments)

**AC-DASH-005: Role-based Views**
- ✅ Super Admin & Asset Admin: Company-wide dashboard
- ✅ Asset Officer, IT Support: Department-level dashboard
- ✅ Manager: Department atau Branch-level dashboard
- ✅ Finance: Financial metrics dashboard (total value, depreciation, maintenance costs)

### Technical Notes
- Firestore aggregation queries untuk counts
- Cloud Functions untuk pre-compute heavy metrics (daily job)
- Cached metrics stored di `/dashboardCache/{cacheKey}`
- Real-time listeners untuk critical metrics (active borrowings, pending approvals)
- Charts: Flutter package `fl_chart`

---

## 4.10 Notification System

### Priority: P0 (Must Have)

### Deskripsi
Push notifications dan in-app notifications untuk berbagai events.

### User Stories

**US-NOTIF-001: Receive Notifications**
```
Sebagai user,
Saya ingin menerima notifikasi untuk events yang relevan,
Supaya saya tidak miss informasi penting.
```

**US-NOTIF-002: Notification Preferences**
```
Sebagai user,
Saya ingin mengatur jenis notifikasi yang saya terima,
Supaya saya tidak terganggu dengan notifikasi yang tidak relevan.
```

### Acceptance Criteria

**AC-NOTIF-001: Notification Types**
- ✅ **Borrow Request** → Manager (push + in-app)
- ✅ **Borrow Approved/Rejected** → Requester (push + in-app)
- ✅ **Asset Transferred** → Old & New responsible person (in-app)
- ✅ **Asset Overdue** → Borrower (push + in-app)
- ✅ **Overdue Escalation** → Manager & Super Admin (push + in-app)
- ✅ **Maintenance Scheduled** → Responsible person (in-app, 7 days before)
- ✅ **Maintenance Due** → IT Support/Asset Officer (push, 1 day before)
- ✅ **Asset Status Changed** → Responsible person (in-app)

**AC-NOTIF-002: Notification Center**
- ✅ Bell icon dengan unread badge
- ✅ Notification list: icon, title, message, timestamp
- ✅ Tap notification → navigate ke relevant screen
- ✅ Mark as read (individual atau mark all)
- ✅ Filter by type atau date
- ✅ Notification retention: 30 days

**AC-NOTIF-003: Preferences**
- ✅ Settings page: toggle per notification type
- ✅ Default: all enabled
- ✅ Cannot disable critical notifications (overdue escalation)
- ✅ Quiet hours (optional): mute notifications during specified hours

### Technical Notes
- Firebase Cloud Messaging (FCM) untuk push notifications
- Collection: `/notifications/{notificationId}`
- Fields: userId, type, title, message, data (untuk navigation), read, createdAt
- Cloud Functions trigger untuk send notifications on events
- FCM token stored di user document

---

## 4.11 AI Assistant (Natural Language Query)

### Priority: P0 (Must Have)

### Deskripsi
Asisten berbasis AI yang dapat menjawab pertanyaan tentang aset menggunakan natural language, powered by Firebase Genkit + Google Gemini.

### User Stories

**US-AI-001: Ask Questions About Assets**
```
Sebagai Manager,
Saya ingin bertanya "Berapa laptop yang belum maintenance selama 1 tahun?",
Supaya saya mendapatkan jawaban dengan instant.
```

**US-AI-002: Query Multiple Conditions**
```
Sebagai Asset Officer,
Saya ingin bertanya dengan kondisi kompleks misalnya "Aset apa saja di Department IT yang sedang dipinjam?",
Supaya saya mendapat informasi yang detail.
```

### Acceptance Criteria

**AC-AI-001: Assistant Interface**
- ✅ Chat-like UI: message bubbles, input text box, send button
- ✅ Input support: free-text natural language queries
- ✅ Response: AI-generated answer dengan context (data summary, count, etc)
- ✅ Response time: <5 seconds (target KPI)
- ✅ Error handling: graceful fallback jika AI response invalid
- ✅ Conversation history saved (last 10 conversations per user)

**AC-AI-002: Query Examples (Supported Patterns)**
- ✅ "Berapa total aset IT?" → Returns: count + percentage
- ✅ "Aset mana saja yang belum maintenance lebih dari 6 bulan?" → Returns: list dengan detail
- ✅ "Berapa nilai total aset di Department Finance?" → Returns: sum dengan kategori breakdown
- ✅ "Siapa yang paling sering meminjam aset?" → Returns: person name + count
- ✅ "Aset apa yang sedang dalam maintenance?" → Returns: list dengan detail
- ✅ "Berapa rata-rata umur aset kategori Vehicle?" → Returns: average age + stats
- ✅ "Show overdue borrowings" → Returns: list dengan overdue duration

**AC-AI-003: AI Integration**
- ✅ Use Firebase Genkit + Gemini dengan system prompt yang terstruktur
- ✅ System prompt instructs Gemini to:
  - Always respond in Bahasa Indonesia
  - Provide factual answers based on asset data
  - Suggest follow-up questions jika perlu
  - Admit uncertainty jika data not clear
- ✅ Query transformation: convert natural language → Firestore query parameters
- ✅ Data retrieval: fetch relevant assets dari Firestore sesuai user's access level
- ✅ Response formatting: structured answer dengan count, summary, examples

**AC-AI-004: Context & Permissions**
- ✅ AI only returns data yang user authorized untuk akses (department, role)
- ✅ Asset Officer: only department's assets
- ✅ Manager: own department assets
- ✅ Finance: all assets (view only)
- ✅ Super Admin & Asset Admin: all assets

**AC-AI-005: Fallback & Error Handling**
- ✅ If query tidak dapat di-interpret: "Maaf, saya tidak memahami pertanyaan. Bisa rephrase?"
- ✅ If data tidak ditemukan: "Data yang Anda cari tidak ditemukan."
- ✅ If API error: "Terjadi kesalahan. Silakan coba lagi nanti."
- ✅ Rate limiting: max 10 queries per user per hour

### Technical Notes
- Backend: Cloud Function dengan Genkit integration
- LLM: Google Gemini (1.5 Flash recommended untuk cost optimization)
- System prompt: detailed instructions untuk struktured data querying
- Execution flow: user query → Genkit flow → Firestore data fetch → Gemini processing → formatted response
- Caching: cache frequent queries hasil (24 hours TTL)
- Cost monitoring: track Gemini API calls, implement quota per user

### Dependencies
- Firebase Genkit setup
- Google Gemini API key (via Firebase)
- Cloud Functions with Node.js 20+
- Firestore query optimization

---

## 4.12 AI Report Generator

### Priority: P0 (Must Have)

### Deskripsi
Generate laporan aset secara otomatis berdasarkan natural language prompt, dengan output PDF atau Excel.

### User Stories

**US-REPORT-001: Generate Report via Prompt**
```
Sebagai Asset Administrator,
Saya ingin generate laporan dengan mengetik "Buatkan laporan aset bulan ini",
Supaya saya tidak perlu manualitas atau ribet set filter.
```

**US-REPORT-002: Scheduled Reports**
```
Sebagai Manager,
Saya ingin setup automated reports yang dikirim ke email setiap bulan,
Supaya saya selalu update tanpa perlu generate manual.
```

### Acceptance Criteria

**AC-REPORT-001: Report Generation Interface**
- ✅ Text input: "Buatkan laporan..."
- ✅ Preset prompts (buttons): Monthly Report, Department Report, Maintenance Report, Borrow Report, Asset Value Report
- ✅ Generate button + loading indicator
- ✅ Generation time: <30 seconds (target KPI)
- ✅ Output formats: PDF (default), Excel

**AC-REPORT-002: Report Content**
- ✅ Title & date
- ✅ Executive Summary (total assets, status breakdown, key metrics)
- ✅ Detailed tables dengan columns relevant ke report type
- ✅ Charts & visualizations (if applicable)
- ✅ Footnotes & disclaimers
- ✅ Generated by & timestamp

**AC-REPORT-003: Report Templates**
- ✅ **Monthly Report**: Total assets, by category, by status, new assets, disposal, maintenance summary
- ✅ **Department Report**: Assets per department, responsible persons, values, status breakdown
- ✅ **Maintenance Report**: Completed maintenance, scheduled, overdue, cost summary, vendor breakdown
- ✅ **Borrow Report**: Total borrowed, active, overdue, top borrowers, top borrowed assets
- ✅ **Asset Value Report**: Total value, depreciation, by category, by department, trend

**AC-REPORT-004: Scheduled Reports**
- ✅ Report settings: frequency (Daily/Weekly/Monthly), recipients, report type, export format
- ✅ Schedule management page: list, create, edit, delete, enable/disable
- ✅ Email delivery: PDF/Excel attachment, sent on specified time
- ✅ Auto-send jika delivery gagal: retry 3x dengan interval 1 hour

**AC-REPORT-005: Report History**
- ✅ All generated reports stored untuk reference
- ✅ List view: report type, generated date, generated by, file link
- ✅ Download/resend capability
- ✅ Retention: 6 months (auto-delete older reports)

### Technical Notes
- Report generation: Cloud Function dengan Genkit + Gemini
- PDF generation: `puppeteer` atau `pdfkit` library
- Excel generation: `exceljs` library
- Email sending: Firebase Cloud Function + SendGrid atau similar
- Storage: Firebase Storage untuk report files
- Scheduled execution: Cloud Scheduler + Cloud Function

---

## 4.13 AI Insights & Predictions

### Priority: P0 (Must Have)

### Deskripsi
Insights dan predictive analytics untuk membantu pengambilan keputusan manajemen aset.

### User Stories

**US-INSIGHT-001: Asset Utilization Insights**
```
Sebagai Manager,
Saya ingin tahu aset mana saja yang jarang digunakan,
Supaya saya bisa optimize atau disposal aset yang tidak perlu.
```

**US-INSIGHT-002: Maintenance Prediction**
```
Sebagai IT Support,
Saya ingin tahu aset mana saja yang akan butuh maintenance segera,
Supaya saya bisa schedule preventive maintenance.
```

**US-INSIGHT-003: Asset Health Score**
```
Sebagai Manager,
Saya ingin melihat health score untuk setiap aset,
Supaya saya tahu mana yang butuh perhatian.
```

### Acceptance Criteria

**AC-INSIGHT-001: Rarely Used Assets**
- ✅ Assets tidak dipinjam dalam 90 hari (configurable)
- ✅ Show: asset name, last usage, department, value
- ✅ AI suggestion: "Aset ini dapat di-dispose atau dialihkan"
- ✅ Action: mark untuk review/disposal

**AC-INSIGHT-002: High Maintenance Costs**
- ✅ Calculate maintenance cost trend per asset category
- ✅ Identify assets dengan cost >threshold (configurable %, default: 30% of purchase price annually)
- ✅ Show: asset name, total cost (YTD), frequency, trend
- ✅ AI suggestion: "Pertimbangkan penggantian daripada terus maintenance"

**AC-INSIGHT-003: Maintenance Prediction**
- ✅ Rule-based atau ML model untuk predict maintenance timing
- ✅ Based on: age, maintenance history, usage frequency
- ✅ Show: asset name, predicted maintenance date, confidence level
- ✅ Create auto-reminder 2 weeks before predicted date

**AC-INSIGHT-004: Asset Health Score**
- ✅ Score formula: 100 - (age factor + maintenance factor + usage factor)
- ✅ Display as: number (0-100) + color indicator (red/yellow/green)
- ✅ Show on dashboard sebagai average score
- ✅ Breakdown per category/department

**AC-INSIGHT-005: Insights Page Layout**
- ✅ Cards untuk setiap insight type (Rarely Used, High Maintenance Cost, Maintenance Prediction, Health Score)
- ✅ Each card shows: metric value, trend (↑/↓), count of affected assets, "View Details" button
- ✅ Details page: list semua affected assets dengan recommendations

**AC-INSIGHT-006: Data Refresh**
- ✅ Insights recalculated daily (nightly at 2 AM)
- ✅ Cache results di Firestore
- ✅ Real-time update jika significant change detected

### Technical Notes
- Insights calculation: Cloud Function triggered by Cloud Scheduler (daily job)
- Storage: `/insights/{month-year}` collection untuk historical tracking
- Prediction model: rule-based untuk MVP (dapat upgrade ke ML model di V2)
- Caching: 24-hour cache dengan version control

---

# 5. Firestore Data Models

## 5.1 Collections Overview

```
SkuyAssets Database Structure:

companies/
  {companyId}/
    - name: string
    - logo: string (URL)
    - address: string
    - phone: string
    - email: string
    - createdAt: timestamp
    - updatedAt: timestamp

departments/
  {departmentId}/
    - companyId: string
    - name: string
    - code: string (unique)
    - description: string
    - headId: string (userId reference)
    - createdAt: timestamp

users/
  {userId}/
    - email: string
    - displayName: string
    - departmentId: string
    - role: string (SUPER_ADMIN, ASSET_ADMIN, ASSET_OFFICER, etc)
    - phone: string
    - profilePicture: string (URL)
    - fcmToken: string
    - status: string (ACTIVE, INACTIVE)
    - createdAt: timestamp
    - lastLogin: timestamp

categories/
  {categoryId}/
    - name: string
    - code: string (unique)
    - icon: string (emoji atau icon name)
    - description: string
    - createdAt: timestamp

assets/
  {assetId}/
    - assetCode: string (unique)
    - name: string
    - description: string
    - categoryId: string
    - departmentId: string
    - responsiblePersonId: string
    - brand: string
    - model: string
    - serialNumber: string (unique per category)
    - purchaseDate: date
    - purchasePrice: number
    - warrantyUntil: date
    - supplier: string
    - location: string
    - status: string (AVAILABLE, IN_USE, BORROWED, MAINTENANCE, RETIRED, DELETED)
    - qrCode: string (base64 data URL)
    - images: array[{url: string, uploadedAt: timestamp}]
    - createdAt: timestamp
    - updatedAt: timestamp
    - createdBy: string (userId)
    - updatedBy: string (userId)

  {assetId}/transfers/
    {transferId}/
      - fromDepartmentId: string
      - toDepartmentId: string
      - fromPersonId: string
      - toPersonId: string
      - transferDate: date
      - reason: string
      - transferredBy: string
      - createdAt: timestamp

  {assetId}/borrowTransactions/
    {transactionId}/
      - borrowerId: string
      - approverId: string
      - borrowDate: date
      - dueDate: date
      - returnDate: date (null jika belum di-return)
      - status: string (PENDING, APPROVED, REJECTED, COMPLETED)
      - condition: string (GOOD, FAIR, DAMAGED)
      - purpose: string
      - notes: string
      - returnPhotos: array[string] (URLs)
      - createdAt: timestamp
      - requestDate: timestamp

  {assetId}/maintenanceRecords/
    {recordId}/
      - type: string (PREVENTIVE, CORRECTIVE, EMERGENCY)
      - maintenanceDate: date
      - scheduledDate: date
      - status: string (SCHEDULED, IN_PROGRESS, COMPLETED)
      - vendorName: string
      - vendorPhone: string
      - vendorEmail: string
      - cost: number
      - description: string
      - notes: string
      - documents: array[{url: string, type: string}]
      - nextMaintenanceDate: date
      - recurrence: string (NONE, MONTHLY, QUARTERLY, YEARLY)
      - loggedBy: string
      - completedBy: string
      - createdAt: timestamp

notifications/
  {notificationId}/
    - userId: string
    - type: string (BORROW_REQUEST, BORROW_APPROVED, ASSET_TRANSFERRED, etc)
    - title: string
    - message: string
    - data: object {assetId, transactionId, etc}
    - read: boolean
    - readAt: timestamp
    - createdAt: timestamp

aiLogs/
  {logId}/
    - userId: string
    - query: string
    - response: string
    - processingTime: number (ms)
    - tokensUsed: number
    - cost: number
    - status: string (SUCCESS, ERROR)
    - errorMessage: string
    - createdAt: timestamp

dashboardCache/
  {cacheKey}/
    - data: object (metrics)
    - updatedAt: timestamp
    - expiresAt: timestamp

insights/
  {insightMonth}/
    - monthYear: string (YYYY-MM)
    - rarelyUsedAssets: array[{assetId, lastUsageDate}]
    - highMaintenanceAssets: array[{assetId, totalCost, yearToDate}]
    - maintenancePredictions: array[{assetId, predictedDate, confidence}]
    - assetHealthScores: array[{assetId, score, category}]
    - generatedAt: timestamp
```

---

# 6. Technical Architecture & API Design

## 6.1 System Architecture Overview

```
┌─────────────────────────────────────────────────────────────────┐
│                      CLIENT (Flutter App)                        │
│  iOS & Android | QR Scanner | Offline Cache | Local Storage    │
└────────────────────┬──────────────────────────────────────────────┘
                     │ (REST/Firestore)
┌────────────────────▼──────────────────────────────────────────────┐
│                  FIREBASE SERVICES                                │
│  ┌──────────────┐  ┌──────────────┐  ┌──────────────────────┐   │
│  │ Authentication │ │ Cloud Firestore  │ │ Firebase Storage   │   │
│  └──────────────┘  └──────────────┘  └──────────────────────┘   │
│  ┌──────────────────┐  ┌──────────────────┐  ┌──────────┐       │
│  │ Cloud Functions  │  │ Cloud Messaging  │  │ Analytics│       │
│  └──────────────────┘  └──────────────────┘  └──────────┘       │
└────────────────────┬───────────────────────────────────────────────┘
                     │
┌────────────────────▼──────────────────────────────────────────────┐
│              FIREBASE GENKIT + GOOGLE GEMINI                       │
│  - Natural Language Processing                                   │
│  - Report Generation                                             │
│  - Insights & Predictions                                        │
└────────────────────────────────────────────────────────────────────┘
```

## 6.2 Core API Endpoints

### Authentication Endpoints
```
POST /auth/register
- Body: email, password, fullName
- Response: {uid, token, user}

POST /auth/login
- Body: email, password
- Response: {uid, token, user}

POST /auth/logout
- Response: {success}

POST /auth/resetPassword
- Body: email
- Response: {success}
```

### Asset Management Endpoints
```
GET /assets
- Query: departmentId, status, category, page, limit
- Response: {items: [...], total, hasMore}

GET /assets/{assetId}
- Response: asset document with full details

POST /assets
- Body: {name, category, department, status, ...}
- Response: {assetId, qrCode, assetCode}

PUT /assets/{assetId}
- Body: {name, status, location, ...}
- Response: {success, updatedAt}

DELETE /assets/{assetId}
- Response: {success}

POST /assets/{assetId}/generateQR
- Response: {qrCode}

POST /assets/import
- Body: FormData (CSV file)
- Response: {imported: number, errors: [...]}
```

### Borrow Endpoints
```
POST /borrows/request
- Body: {assetId, dueDate, purpose}
- Response: {transactionId, status}

POST /borrows/{transactionId}/approve
- Body: {approverId}
- Response: {success, notification sent}

POST /borrows/{transactionId}/reject
- Body: {reason}
- Response: {success, notification sent}

POST /borrows/{transactionId}/return
- Body: {condition, notes, photos[]}
- Response: {success, assetStatus}

GET /borrows/active
- Response: {items: [...], overdue: number}
```

### Maintenance Endpoints
```
POST /maintenance/log
- Body: {assetId, type, date, vendor, cost, status, ...}
- Response: {recordId, success}

POST /maintenance/schedule
- Body: {assetId, type, scheduledDate, recurrence, ...}
- Response: {scheduleId, success}

GET /maintenance/asset/{assetId}
- Response: {items: [...], totalCost, stats}
```

### AI Endpoints
```
POST /ai/assistant
- Body: {query, userDepartmentId (untuk context)}
- Response: {answer, sources, suggestedFollowUps}

POST /ai/generateReport
- Body: {type, filters, format: "pdf"|"excel"}
- Response: {reportId, downloadUrl}

GET /ai/insights
- Query: monthYear (optional)
- Response: {rarelyUsed, highMaintenance, predictions, healthScores}
```

### Dashboard Endpoints
```
GET /dashboard/metrics
- Query: departmentId (optional)
- Response: {totalAssets, byStatus, byCategory, totalValue, ...}

GET /dashboard/activities
- Query: limit (default 20)
- Response: {items: [{timestamp, action, details}]}
```

---

# 7. User Experience & Key Flows

## 7.1 Key User Journeys

### Journey 1: Asset Officer - Audit Workflow
```
1. Open app → Dashboard
2. Tap "Start Audit"
3. For each location:
   a. Scan QR/Barcode dengan camera
   b. App shows: Asset detail, expected status
   c. Update status/location jika berbeda
   d. Take photo jika ada damage
4. Complete audit → Generate audit report
5. Sync data ke cloud
```

### Journey 2: Staff - Borrow Asset
```
1. Tap "Borrow Asset" menu
2. Search atau scan asset
3. Fill borrow form: due date, purpose
4. Submit request
5. Wait for manager approval (notification)
6. Status: "Approved" → asset dapat diambil
7. Scan asset saat pengambilan
8. Return asset: scan QR, fill return form, upload photos jika ada damage
```

### Journey 3: Manager - Decision Making
```
1. Open dashboard
2. Review key metrics: total assets, status distribution, overdue borrowings
3. Ask AI Assistant: "Aset mana yg harus di-replace?"
4. Generate monthly report
5. Review AI Insights untuk maintenance planning
```

### Journey 4: Asset Admin - Monthly Closing
```
1. Generate monthly report via AI
2. Review AI Insights (rarely used, high maintenance)
3. Approve disposal/transfers yang pending
4. Export depreciation report untuk Finance
```

## 7.2 Screen List

### Core Screens:
1. **Splash Screen** - Loading & initial setup
2. **Login Screen** - Email & password
3. **Forgot Password Screen** - Password reset
4. **Dashboard** - Home screen dengan metrics
5. **Assets List Screen** - List dengan search & filter
6. **Asset Detail Screen** - Full detail + history
7. **Create/Edit Asset Screen** - Form
8. **QR Scanner Screen** - Camera + detection
9. **Borrow/Return Flow Screens** (3 screens) - Request, Approval, Return
10. **Maintenance Log Screen** - Log & schedule
11. **AI Assistant Screen** - Chat interface
12. **Reports Screen** - Generate & list
13. **Settings Screen** - Preferences, profile
14. **Notifications Center** - List & manage

---

# 8. Non-Functional Requirements

## 8.1 Performance Requirements

| Metric | Target | Notes |
|--------|--------|-------|
| App Startup Time | <3 seconds | Cold start pada device dengan connectivity |
| Screen Load Time | <2 seconds | Rata-rata screen load setelah navigation |
| Dashboard Load | <2 seconds | KPI dari BRD |
| Search Response | <500ms | Local filter + server search |
| QR Scan Detection | <2 seconds | Dari focus hingga detection |
| AI Response Time | <5 seconds | KPI dari BRD, excluding network latency |
| Report Generation | <30 seconds | KPI dari BRD |
| Image Upload | <5 seconds | Per image 1-2MB |
| Sync Time | <10 seconds | Full sync after offline session |

## 8.2 Scalability & Infrastructure

- **Target Users (MVP)**: 100-500 concurrent users
- **Target Assets**: 10,000-50,000 assets
- **Firestore Quotas**:
  - Writes: 60K per minute (auto-scales)
  - Reads: Auto-scales
  - Delete: Auto-scales
- **Firebase Storage**: Unlimited (pay per GB)
- **Cloud Functions**: Auto-scale based on demand
- **Cost Optimization**:
  - Use Firestore regional index untuk complex queries
  - Implement caching untuk dashboard metrics
  - Batch operations untuk bulk imports
  - Offline-first untuk reduce network calls

## 8.3 Security Requirements

- **Authentication**: Firebase Auth dengan Email/Password (future: SSO)
- **Authorization**: Role-based access control (RBAC) via Firestore Security Rules
- **Data Encryption**: In-transit (HTTPS/TLS), at-rest (Firebase default)
- **Audit Trail**: All changes logged dengan user & timestamp
- **Session Management**:
  - Auto-logout after 30 days inactivity
  - Max 5 login attempts per 15 minutes
- **API Security**:
  - Rate limiting per user: 1000 requests/hour
  - Input validation & sanitization
  - No sensitive data di logs (Firebase Crashlytics)
- **Compliance**:
  - GDPR: data deletion & export capability
  - Indonesia: SOC 2 certification aspirational (Firebase compliant)

## 8.4 Reliability & Availability

- **Target Uptime**: 99.5% (Firebase infrastructure)
- **Backup Strategy**: Firebase automatic backups (customer managed export available)
- **Disaster Recovery**: Firebase multi-region redundancy
- **Error Handling**: Graceful degradation, offline fallback untuk critical features
- **Monitoring**: Firebase Crashlytics + custom error tracking
- **Support**: In-app error reporting untuk users

## 8.5 Offline Capability

- **MVP Offline Strategy**: Read-only cache
  - Asset list & details cached locally
  - Sync queue untuk pending operations
  - Background sync saat connectivity restored
- **Scope**:
  - View assets ✅
  - Search cached assets ✅
  - View asset detail ✅
  - Scan QR (offline) ✅
  - Create asset (queue) ✅
  - Edit asset (queue) ✅
  - Borrow/Return (queue) ✅

---

# 9. Success Metrics & KPIs

## 9.1 Business KPIs (dari BRD)

| KPI | Target | Baseline | Timeline | Owner |
|-----|--------|----------|----------|-------|
| Seluruh aset memiliki QR Code | 100% | 0% | 3 bulan | Asset Admin |
| Waktu pencarian aset | <30 detik | 5-10 menit | 1 bulan | Asset Officer |
| Waktu audit | Berkurang >50% | 2 minggu | 3 bulan | Asset Officer |
| AI Response Time | <5 detik | N/A | 2 bulan | Engineering |
| Dashboard Loading | <2 detik | N/A | 1 bulan | Engineering |
| Asset Data Accuracy | >98% | 80% | 2 bulan | Asset Admin |

## 9.2 Product KPIs

| KPI | Target | Notes |
|-----|--------|-------|
| User Adoption | 80% of employees | Adopsi aktif dalam 6 bulan |
| Daily Active Users (DAU) | 60% | Active usage harian |
| Borrow Request Approval Time | <1 jam | Dari request sampai approval |
| Maintenance Schedule Compliance | 95% | Maintenance dilakukan on-time |
| Overdue Borrow Reduction | 80% | Dari baseline |
| Report Generation Time | <30 seconds | <2 menit untuk large reports |

## 9.3 Technical KPIs

| KPI | Target | Notes |
|-----|--------|-------|
| App Crash Rate | <0.1% | Monitored via Crashlytics |
| Average Session Duration | >10 minutes | User engagement |
| API Response Time (p95) | <500ms | Firestore queries |
| QR Scan Success Rate | 99% | First-time scan success |
| Data Sync Reliability | 99.9% | Offline-online sync success |

## 9.4 Tracking Implementation

- **Analytics Events**: Firebase Analytics untuk track user actions (asset view, search, borrow, etc)
- **Custom Metrics**: Cloud Function scheduled jobs untuk compute KPIs daily
- **Dashboards**: Internal analytics dashboard untuk monitor KPIs
- **Alerts**: Setup alerts jika KPI below target untuk quick intervention

---

# 10. Out of Scope (MVP)

| Feature | Reason | Target Version |
|---------|--------|-----------------|
| ERP Integration | Complex, dapat dilakukan post-launch | V3 |
| SAP/Oracle Integration | Sama dengan ERP | V3 |
| Offline-first full sync | MVP fokus read-only offline | V2 |
| Multi-company support | MVP single-tenant untuk simplicity | V2 |
| Approval workflow (complex) | MVP hanya simple approval | V2 |
| Asset booking system | Future feature | V2 |
| RFID tracking | Require hardware | V4 |
| IoT sensors | Require hardware | V4 |
| AI Vision (detect asset dari foto) | Kompleks, low priority | V3 |
| OCR asset recognition | Low priority | V3 |
| Web admin panel | MVP mobile-first | V2 |
| Procurement module | Separate system | Future |
| Accounting integration | Finance team needs | V3 |
| Payroll integration | HR system | Out of scope |

---

# 11. Risks, Constraints & Assumptions

## 11.1 Key Risks

| Risk | Probability | Impact | Mitigation |
|------|-------------|--------|-----------|
| Data aset tidak lengkap | High | High | Validasi on input, admin review queue |
| User resistance to change | Medium | High | Antarmuka sederhana, training, support |
| AI cost meningkat dengan scale | Medium | Medium | Quota limiting, caching, monitoring |
| AI response quality inconsistent | Medium | Medium | Prompt engineering, data validation, human review |
| QR code quality issues (damaged) | Low | Low | Re-generate option, backup barcode |
| Performance degradation with scale | Low | Medium | Index optimization, caching, load testing |

## 11.2 Constraints

### Technical Constraints:
- MVP mobile-only (no web admin panel)
- Single company per deployment
- Firebase free tier limitations (eventual upgrade to paid tier)
- Firebase Genkit cost metering required
- QR generation must handle bulk operations efficiently

### Business Constraints:
- MVP timeline: 3 months target
- Budget limitations for paid Firebase services
- AI model limited to Gemini (no fine-tuning in MVP)
- No RFID/IoT hardware support in MVP

## 11.3 Assumptions

### User Assumptions:
- Seluruh user memiliki perangkat mobile (Android/iOS)
- User memiliki basic mobile app literacy
- Internet connectivity tersedia untuk sync (assumed, offline fallback provided)

### Technical Assumptions:
- Firebase dapat handle expected load (100-500 concurrent users)
- Gemini API response time <5 seconds konsisten
- Flutter dapat compile untuk iOS & Android tanpa issue

### Business Assumptions:
- Company siap melakukan data migration
- QR printing capability tersedia (user menggunakan printer mereka)
- Manager akan actively approve borrow requests
- User engaged dengan dashboard & insights

---

# 12. Timeline & Milestones (MVP)

## 12.1 Phase-Based Roadmap

### Phase 1: Foundation (Week 1-4)
**Focus**: Core infrastructure & authentication
- Setup Firebase project & Firestore schema
- Implement user authentication & RBAC
- Create company & department management
- Design & implement base UI components
- Setup CI/CD pipeline

**Deliverables**:
- ✅ Firestore database initialized
- ✅ User login/registration working
- ✅ Authentication tested
- ✅ CI/CD pipeline ready

**Team**: Backend (2), Frontend (2), DevOps (1)

### Phase 2: Core Features (Week 5-8)
**Focus**: Asset management, QR code, scanning
- Implement asset CRUD operations
- QR code generation & storage
- QR scanner functionality
- Asset list & search
- Dashboard basic metrics
- Image upload capability

**Deliverables**:
- ✅ Asset management MVP
- ✅ QR generation & scanning tested
- ✅ Dashboard with 5+ metrics
- ✅ Internal QA testing

**Team**: Backend (2), Frontend (2), QA (1)

### Phase 3: Transactions & AI (Week 9-10)
**Focus**: Borrow/return, maintenance, basic AI
- Implement borrow/return workflow
- Maintenance logging & scheduling
- Implement AI Assistant (natural language queries)
- Simple report generation
- Push notifications setup

**Deliverables**:
- ✅ Borrow/return workflow complete
- ✅ AI Assistant MVP working
- ✅ Report generation basic
- ✅ Notifications system tested

**Team**: Backend (2), Frontend (2), AI/ML (1), QA (1)

### Phase 4: Refinement & Launch (Week 11-12)
**Focus**: Performance optimization, testing, launch prep
- Performance tuning & optimization
- Comprehensive QA & bug fixes
- User documentation & training materials
- App store submission (iOS/Android)
- Soft launch to pilot users

**Deliverables**:
- ✅ App launched di app stores
- ✅ Pilot user group active
- ✅ All KPIs monitored
- ✅ Support processes ready

**Team**: Backend (1), Frontend (1), QA (2), Product (1)

## 12.2 Sprint Breakdown

### Sprint 1 (Week 1-2): Foundation
- Firebase setup & Firestore schema
- Auth screens & logic
- User & role management
- Base component library

### Sprint 2 (Week 3-4): Organization & Assets
- Company & department setup screens
- Asset CRUD forms
- Asset list view with filter
- QR generation backend

### Sprint 3 (Week 5-6): Scanning & Dashboard
- QR scanner implementation
- Asset detail view
- Dashboard basic layout
- Metrics calculation

### Sprint 4 (Week 7-8): Transactions
- Borrow request flow
- Maintenance logging
- Transfer functionality
- Transaction history

### Sprint 5 (Week 9): AI & Reporting
- AI Assistant integration
- Report generation
- Basic insights
- Notification system

### Sprint 6 (Week 10-12): Polish & Launch
- Performance optimization
- Comprehensive testing
- Bug fixes & refinements
- App store submission
- Monitoring & support setup

## 12.3 Resource Plan

### Team Composition (Recommended)
- **Backend Engineers**: 2 FTE (Firebase, Cloud Functions, AI)
- **Frontend Engineers**: 2 FTE (Flutter, UI/UX)
- **QA Engineers**: 1-2 FTE (Testing, automation)
- **AI/ML Engineer**: 0.5 FTE (Genkit integration, prompt engineering)
- **DevOps**: 0.5 FTE (CI/CD, infrastructure)
- **Product Manager**: 1 FTE (This document owner)
- **Designer**: 1 FTE (UI/UX design)

**Total**: ~8 FTE team

## 12.4 Key Milestones & Gates

| Milestone | Date | Gate/Success Criteria |
|-----------|------|----------------------|
| Foundation Complete | Week 4 | Auth working, Firestore ready |
| Core Features Ready | Week 8 | Asset CRUD, QR, Dashboard MVP |
| AI Integration Done | Week 10 | Assistant & reports working |
| MVP Launch | Week 12 | App stores live, pilot users active |
| First Metrics Review | Week 14 | KPI baseline established |

---

# 13. Glossary

| Term | Definition |
|------|-----------|
| Asset | Item owned by company yang di-track (laptop, furniture, vehicle, etc) |
| QR Code | Machine-readable barcode with asset information |
| Borrow Transaction | Record saat user meminjam dan mengembalikan asset |
| Maintenance Record | Log dari servis/maintenance yang dilakukan pada asset |
| Transfer | Movement of asset ownership antar department/person |
| Status | Current state of asset (Available, Borrowed, In Maintenance, etc) |
| RBAC | Role-Based Access Control - permission system berdasarkan user role |
| Firestore | Google Cloud Database untuk MVP |
| Genkit | Firebase AI Framework untuk Gemini integration |
| KPI | Key Performance Indicator - metric untuk measure success |

---

# 14. Appendix: Sample User Flows

## A. Asset Officer - Daily Inventory Check

```
1. Open app
2. Tap "Dashboard" → View metrics
3. Tap "Scan QR" button
4. Scan 5 assets di warehouse
5. For each asset:
   - Confirm location: "Gudang Lantai 2"
   - Confirm condition: "Good"
6. After scanning 5 assets:
   - Tap "Complete Audit"
   - App syncs data
7. Return to dashboard → updated metrics
```

## B. Manager - Weekly Asset Review

```
1. Open app
2. View Dashboard
3. Tap "Pending Approvals" → 3 borrow requests
4. Review each request:
   - Asset: Laptop Dell XPS-001
   - Requester: Budi (Finance)
   - Purpose: Training
   - Tap "Approve"
5. Tap "Overdue Borrowings" → 1 item
6. Tap "Send Reminder" to borrower
7. Tap "Generate Report" → Select "Weekly Asset Report"
8. Report generated (PDF) → Tap "Email to Team"
```

## C. Asset Admin - Monthly Data Migration

```
1. Open app
2. Tap "Settings" → "Import/Export"
3. Select "Import from CSV"
4. Upload "assets_july.csv" file
5. App shows preview: "100 assets akan diimport"
6. Tap "Confirm Import"
7. Progress: importing...
8. Success: "95 assets imported, 5 errors"
9. Tap "Download Error Report"
10. Fix issues in CSV, re-upload
```

---

# 15. Document Control

| Field | Value |
|-------|-------|
| Document Title | Product Requirements Document - SkuyAssets Pro |
| Version | 1.0 |
| Status | Draft |
| Last Updated | July 2, 2026 |
| Owner | Product Manager |
| Reviewers | Engineering Lead, Design Lead |
| Approval Date | [Pending] |
| Next Review | August 2, 2026 |

**Document History:**
- v1.0 (July 2, 2026): Initial draft from BRD

---

**END OF DOCUMENT**

