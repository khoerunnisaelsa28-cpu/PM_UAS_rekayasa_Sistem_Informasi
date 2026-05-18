**Platform Pemesanan Jasa Lokal Kecamatan Bojongloa Kaler Berbasis Mobile & API**

> Version 1.0 approved (UTS)

**Disusun Oleh:**

| Nama |
|------|
| Nabila Asana Alecia |
| Aldy Ramadany |
| Tetep Safarudin |
| Nabil Ramadhan |
| Fatin Asyifa Nurrizky JenPutri |
| Fazna Laisal Ramadan |
| R. Elsa Putri |
| M. Fajar Nurjaman |

**PROGRAM STUDI SISTEM INFORMASI**
**FAKULTAS ILMU KOMPUTER DAN SISTEM INFORMASI**
**UNIVERSITAS KEBANGSAAN REPUBLIK INDONESIA**
**TAHUN 2026**

---

## Table of Contents

[Revision History](#revision-history)

1. [Introduction](#1-introduction)
   - 1.1 [Purpose](#11-purpose)
   - 1.2 [Document Conventions](#12-document-conventions)
   - 1.3 [Intended Audience and Reading Suggestions](#13-intended-audience-and-reading-suggestions)
   - 1.4 [Product Scope](#14-product-scope)
   - 1.5 [References](#15-references)
2. [Overall Description](#2-overall-description)
   - 2.1 [Product Perspective](#21-product-perspective)
   - 2.2 [Product Functions](#22-product-functions)
   - 2.3 [User Classes and Characteristics](#23-user-classes-and-characteristics)
   - 2.4 [Operating Environment](#24-operating-environment)
   - 2.5 [Design and Implementation Constraints](#25-design-and-implementation-constraints)
   - 2.6 [User Documentation](#26-user-documentation)
   - 2.7 [Assumptions and Dependencies](#27-assumptions-and-dependencies)
3. [External Interface Requirements](#3-external-interface-requirements)
   - 3.1 [User Interfaces](#31-user-interfaces)
   - 3.2 [Hardware Interfaces](#32-hardware-interfaces)
   - 3.3 [Software Interfaces](#33-software-interfaces)
   - 3.4 [Communications Interfaces](#34-communications-interfaces)
4. [System Features](#4-system-features)
   - 4.1 [Authentication & Authorization](#41-authentication--authorization)
   - 4.2 [Provider Management](#42-provider-management)
   - 4.3 [Service Catalog & Search](#43-service-catalog--search)
   - 4.4 [Order Lifecycle](#44-order-lifecycle)
   - 4.5 [Payment (DP & Final) via QRIS](#45-payment-dp--final-via-qris)
   - 4.6 [Notifications](#46-notifications)
   - 4.7 [Rating & Review](#47-rating--review)
   - 4.8 [Treasurer Monitoring](#48-treasurer-monitoring)
5. [Other Nonfunctional Requirements](#5-other-nonfunctional-requirements)
   - 5.1 [Performance Requirements](#51-performance-requirements)
   - 5.2 [Safety Requirements](#52-safety-requirements)
   - 5.3 [Security Requirements](#53-security-requirements)
   - 5.4 [Software Quality Attributes](#54-software-quality-attributes)
   - 5.5 [Business Rules](#55-business-rules)
6. [Other Requirements](#6-other-requirements)

[Appendix A: Glossary](#appendix-a-glossary)

[Appendix B: Analysis Models](#appendix-b-analysis-models)

[Appendix C: To Be Determined List](#appendix-c-to-be-determined-list)

---

## Revision History

| Name | Date | Reason for Change | Version |
|------|------|-------------------|---------|
| Kelompok A2 | 2026-03-23 | Initial SRS for TukangDekat | 1.0 |

---

## 1. Introduction

### 1.1 Purpose

Dokumen ini menetapkan kebutuhan perangkat lunak untuk **TukangDekat**, sebuah sistem informasi yang menghubungkan warga atau pelanggan dengan penyedia jasa lokal (tukang atau teknisi) di **Kecamatan Bojongloa Kaler, Bandung**, dengan tujuan memudahkan proses pemesanan dan pembayaran.

### 1.2 Document Conventions

- Kebutuhan fungsional diberi kode `FR-xx`.
- Kebutuhan nonfungsional diberi kode `NFR-xx`.
- Aturan bisnis diberi kode `BR-xx`.
- Prioritas: **High**, **Medium**, **Low**.
- Format tanggal: `YYYY-MM-DD`.

### 1.3 Intended Audience and Reading Suggestions

Dokumen ini ditujukan untuk:

- **Dosen / Penguji** — sebagai acuan evaluasi analisis dan desain.
- **Tim Pengembang** — sebagai acuan implementasi backend dan mobile.
- **Tim Pengujian** — sebagai acuan skenario uji.

> Urutan baca yang disarankan: Bagian 1–2 (overview) → Bagian 4 (fitur) → Bagian 3 (interface) → Bagian 5 (nonfungsional) → Lampiran (model).

### 1.4 Product Scope

TukangDekat bertujuan:

- Mempermudah warga memperoleh jasa lokal yang relevan.
- Meningkatkan kesempatan kerja dan pendapatan penyedia jasa lokal.
- Menyediakan alur pemesanan dan pembayaran yang transparan.
- Menyediakan riwayat transaksi dan status order secara real-time.

Kategori jasa yang difokuskan:

| No | Kategori |
|----|----------|
| 1 | Listrik |
| 2 | Plumbing |
| 3 | AC |
| 4 | Bangunan Ringan |
| 5 | Servis Elektronik Rumah |

### 1.5 References

- Panduan Tugas Besar Rekayasa Sistem Informasi Kelas A1.
- [Dokumentasi Laravel](https://laravel.com/docs)
- [Dokumentasi Flutter](https://docs.flutter.dev)
- [Dokumentasi Docker dan Docker Compose](https://docs.docker.com)
- [Dokumentasi n8n](https://docs.n8n.io)
- Dokumentasi payment gateway pendukung QRIS (Midtrans / Xendit) dan webhook.

---

## 2. Overall Description

### 2.1 Product Perspective

Sistem ini menggantikan proses pemesanan jasa yang umumnya dilakukan secara manual (rekomendasi tetangga atau WhatsApp) menjadi sistem terintegrasi yang mendukung pencarian penyedia jasa, pemesanan, pembayaran digital, dan manajemen order.

### 2.2 Product Functions

Fungsi utama sistem:

- Registrasi dan login pengguna berbasis role.
- Pengelolaan profil penyedia jasa dan verifikasi.
- Pencarian dan pemilihan jasa.
- Pembuatan order dan perubahan status.
- Pembayaran DP 50% dan pelunasan 50% melalui QRIS.
- Penerimaan notifikasi status pembayaran dari payment gateway.
- Notifikasi otomatis WhatsApp/email via n8n.
- Rating dan ulasan.
- Monitoring transaksi oleh pengurus dan bendahara.

### 2.3 User Classes and Characteristics

| Role | Deskripsi |
|------|-----------|
| **Customer** (warga/pelanggan) | Memesan jasa, melakukan pembayaran, memberi rating. |
| **Provider** (tukang/teknisi) | Menerima order, mengerjakan, memperbarui status order. |
| **Admin** (pengurus) | Mengelola kategori, memverifikasi provider, memonitor aktivitas. |
| **Treasurer** (bendahara) | Memonitor transaksi pembayaran, rekap pembayaran. |

### 2.4 Operating Environment

- **Mobile:** Android minimal versi 8+.
- **Backend:** Server Linux / VPS.
- **Database:** MySQL atau PostgreSQL.
- **Deployment:** Backend menggunakan Docker dan Docker Compose.
- **Komunikasi:** HTTPS.

### 2.5 Design and Implementation Constraints

- Backend bersifat **API-only**.
- Backend (Laravel, web server, database) harus terisolasi dengan Docker.
- Mobile mengonsumsi API backend.
- Backend harus dapat dihosting (hosting backend saja).
- Sistem wajib terintegrasi dengan pembayaran **QRIS** serta notifikasi via **n8n**.

### 2.6 User Documentation

- Panduan penggunaan customer.
- Panduan penggunaan provider.
- Panduan admin dan bendahara.
- Dokumentasi API.

### 2.7 Assumptions and Dependencies

- Payment gateway menyediakan sandbox dan webhook callback.
- Pengiriman WhatsApp/email bergantung pada konfigurasi provider pada n8n.
- Kebijakan komisi platform dan refund akan ditentukan pada fase implementasi (**TBD**).

---

## 3. External Interface Requirements

### 3.1 User Interfaces

Minimal layar aplikasi:

- Login / Register
- Home + Kategori
- Daftar Provider + Detail
- Form Buat Order (jadwal, alamat, catatan)
- Detail Order + Status
- Halaman Pembayaran (DP dan pelunasan)
- Riwayat Order
- Rating & Ulasan
- Dashboard Admin / Bendahara (role-based)

### 3.2 Hardware Interfaces

Kamera perangkat dapat digunakan untuk unggah foto kerusakan *(opsional)*.

### 3.3 Software Interfaces

| Komponen | Protokol/Format |
|----------|----------------|
| Mobile ↔ Backend | REST API (JSON) |
| Backend ↔ Database | — |
| Backend ↔ Payment Gateway | QRIS + Webhook Callback |
| Backend ↔ n8n | Webhook untuk trigger notifikasi |

### 3.4 Communications Interfaces

- HTTP/HTTPS dengan JSON payload.
- Mekanisme autentikasi token untuk mengakses API.
- Webhook endpoint untuk notifikasi pembayaran.

---

## 4. System Features

### 4.1 Authentication & Authorization

> **Priority:** High

| Kode | Deskripsi |
|------|-----------|
| `FR-01` | Sistem menyediakan registrasi pengguna (customer/provider). |
| `FR-02` | Sistem menyediakan login dan token akses. |
| `FR-03` | Sistem menyediakan logout. |
| `FR-04` | Sistem menerapkan pembatasan akses berdasarkan role. |

### 4.2 Provider Management

> **Priority:** High

| Kode | Deskripsi |
|------|-----------|
| `FR-05` | Provider dapat membuat dan memperbarui profil. |
| `FR-06` | Admin dapat memverifikasi provider. |
| `FR-07` | Admin dapat menonaktifkan provider bila melanggar kebijakan. |

### 4.3 Service Catalog & Search

> **Priority:** High

| Kode | Deskripsi |
|------|-----------|
| `FR-08` | Sistem menyediakan kategori jasa dan daftar provider per kategori. |
| `FR-09` | Customer dapat mencari provider berdasarkan kategori dan kata kunci. |
| `FR-10` | Sistem menampilkan detail provider dan layanan yang tersedia. |

### 4.4 Order Lifecycle

> **Priority:** High

| Kode | Deskripsi |
|------|-----------|
| `FR-11` | Customer dapat membuat order (provider, jadwal, alamat, catatan, foto opsional). |
| `FR-12` | Provider dapat menerima atau menolak order. |
| `FR-13` | Sistem menyimpan status order: `CREATED` → `ACCEPTED` → `IN_PROGRESS` → `COMPLETED` → `CANCELLED` / `CLOSED`. |
| `FR-14` | Provider dapat memulai pengerjaan (`IN_PROGRESS`) hanya jika DP sudah dibayar. |

**State Diagram Status Order:**

```
CREATED → ACCEPTED → IN_PROGRESS → COMPLETED → CLOSED
                  ↘ CANCELLED
```

### 4.5 Payment (DP & Final) via QRIS

> **Priority:** High

| Kode | Deskripsi |
|------|-----------|
| `FR-15` | Sistem membuat tagihan DP sebesar 50% dari estimasi biaya saat order dibuat. |
| `FR-16` | Sistem menghasilkan QRIS untuk pembayaran DP. |
| `FR-17` | Sistem menerima webhook callback pembayaran dari payment gateway dan memperbarui status DP. |
| `FR-18` | Setelah order `COMPLETED` dan `final_price` ditetapkan, sistem membuat tagihan pelunasan sebesar `(final_price - dp_amount)`. |
| `FR-19` | Sistem menghasilkan QRIS untuk pembayaran pelunasan. |
| `FR-20` | Sistem menutup order (`CLOSED`) ketika pelunasan berhasil dibayar. |

### 4.6 Notifications

> **Priority:** Medium

| Kode | Deskripsi |
|------|-----------|
| `FR-21` | Sistem mengirim event notifikasi ke n8n saat: order dibuat, order diterima/ditolak, DP paid, order completed, final paid. |
| `FR-22` | Sistem mengirim notifikasi ke customer/provider melalui WhatsApp dan email (melalui n8n). |

### 4.7 Rating & Review

> **Priority:** Medium

| Kode | Deskripsi |
|------|-----------|
| `FR-23` | Customer dapat memberi rating dan ulasan setelah order selesai. |
| `FR-24` | Sistem menghitung rating rata-rata provider. |

### 4.8 Treasurer Monitoring

> **Priority:** Medium

| Kode | Deskripsi |
|------|-----------|
| `FR-25` | Bendahara dapat melihat daftar transaksi DP dan pelunasan. |
| `FR-26` | Bendahara dapat melihat ringkasan transaksi berdasarkan rentang tanggal. |

---

## 5. Other Nonfunctional Requirements

### 5.1 Performance Requirements

| Kode | Deskripsi |
|------|-----------|
| `NFR-01` | Respons API endpoint umum < 1 detik pada kondisi normal. |
| `NFR-02` | Sistem mendukung minimal 100 order/hari (skala tugas besar). |

### 5.2 Safety Requirements

| Kode | Deskripsi |
|------|-----------|
| `NFR-03` | Sistem menyimpan jejak perubahan status order dan event pembayaran untuk audit. |

### 5.3 Security Requirements

| Kode | Deskripsi |
|------|-----------|
| `NFR-04` | Semua komunikasi menggunakan HTTPS. |
| `NFR-05` | Password disimpan dengan hashing yang aman. |
| `NFR-06` | Webhook payment gateway diverifikasi menggunakan secret/signature. |
| `NFR-07` | Pembatasan akses endpoint berdasarkan role. |

### 5.4 Software Quality Attributes

| Kode | Atribut | Deskripsi |
|------|---------|-----------|
| `NFR-08` | Maintainability | Modul terpisah: auth, catalog, orders, payments, notifications. |
| `NFR-09` | Reliability | Transaksi pembayaran menggunakan database transaction. |
| `NFR-10` | Usability | Proses pemesanan dapat dilakukan dengan langkah yang sederhana. |

### 5.5 Business Rules

| Kode | Aturan |
|------|--------|
| `BR-01` | DP sebesar 50% dari estimasi biaya. |
| `BR-02` | Provider hanya dapat memulai pengerjaan setelah DP dibayar. |
| `BR-03` | Pelunasan dilakukan setelah order selesai. |
| `BR-04` | Komisi platform dan refund policy: **TBD**. |

---

## 6. Other Requirements

- Export laporan transaksi *(opsional)*.
- SLA respon provider *(opsional / TBD)*.

---

## Appendix A: Glossary

| Istilah | Definisi |
|---------|----------|
| **Customer** | Pelanggan yang memesan jasa. |
| **Provider** | Penyedia jasa (tukang/teknisi). |
| **Order** | Pemesanan jasa. |
| **DP** | Uang muka (Down Payment). |
| **QRIS** | QR Code Indonesian Standard — standar pembayaran QR nasional. |
| **Webhook** | Endpoint callback notifikasi pembayaran dari payment gateway. |

---

## Appendix B: Analysis Models

- Use Case Diagram
- Activity Diagram
  <img width="500" height="400" alt="ACTIVITY_DIAGRAM_TUKANG_DEKAT" src="https://github.com/user-attachments/assets/424d67e7-72f8-4e8a-a68a-598d3d4355b9" />
- Rancangan Database
  <img width="500" height="400" alt="ERDDiagram" src="https://github.com/user-attachments/assets/0a9eddac-d065-43a7-893d-8d99cf67e375" />
- Rancangan Arsitektur System
  <img width="500" height="400" alt="arsitektur_sistem_tukangdekat" src="https://github.com/user-attachments/assets/67a1bdd4-7d80-4401-879e-a2ec0720ac45" />
- Rancangan Arsitektur Teknologi
  <img width="500" height="400" alt="arsitektur_teknologi_tukangdekat (1)" src="https://github.com/user-attachments/assets/33f7ba45-fa7b-4a2f-a118-b0547923e7ea" />
- Class Diagram
- Component Diagram
- Deployment Diagram
- Gambaran Kasar UI App Mobile
  [UI Kasar TukangDekat.pdf](https://github.com/user-attachments/files/27321904/UI.Kasar.TukangDekat.pdf)


---

## Appendix C: To Be Determined List

- Payment gateway final yang digunakan (rencana: Midtrans sandbox).
- Komisi platform dan settlement ke provider.
- Refund policy DP bila pembatalan.
- Pembuatan *Sequence Diagram* untuk alur pembayaran dan order.
- Penyusunan *API Contract* dan dokumentasi teknis API untuk integrasi Mobile–Backend.
