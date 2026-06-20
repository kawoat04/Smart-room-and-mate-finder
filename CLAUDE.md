# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Project Overview

**เว็บแอปพลิเคชันระบบค้นหาเพื่อนร่วมห้อง** — a Thai-language PHP web application for finding roommates and rental rooms in Thailand. Developed as a university project (circa 2016) by three students.

**Tech stack**: PHP 5.6 · MySQL/MariaDB · Apache (XAMPP) · Bootstrap 3 · jQuery · JavaScript · CSS

## Repository Structure

This repository does **not** contain PHP source files directly. The actual web application source is distributed only as a compiled Windows installer:

- `Source Code/last.exe` — ~22MB Windows PE32 installer containing the PHP source (extract to get the PHP files)
- `Database/project.sql` — Full MySQL dump with schema and seed data (import to phpMyAdmin as database `project`, charset `utf8_unicode_ci`)
- `Documents/รูปเล่มระบบค้นหาเพื่อนร่วมห้อง.pdf` — Thai-language project documentation
- `manual.docx` — User manual
- `Poster/` — Project poster images
- `Insastaller/Sublime Text Build 3126 x64 Setup.exe` — Text editor installer

## Local Setup

To run the application (Windows + XAMPP):

1. Install XAMPP 5.6.21 and start Apache + MySQL
2. Place the extracted PHP source folder under `C:\xampp\htdocs\`
3. Open phpMyAdmin at `http://localhost/phpmyadmin/`
4. Create database named `project` with collation `utf8_unicode_ci`
5. Execute the SQL in `Database/project.sql` via the SQL tab

**Minimum requirements**: 1GHz CPU, 1GB RAM, DirectX 9

**Browsers supported**: Firefox, Google Chrome

## Database Schema

All tables use `tis620_bin` or `utf8` charset (Thai encoding). Key tables:

| Table | Purpose |
|---|---|
| `users` | User profiles — roommate preferences, location (lat/lng), Facebook ID (`fbid`), rent budget, moving date |
| `room` | Room listings — size, type (`condo`/`flat`/`apartment`/`mansion`/`dormitory`/`rentalhouse`), rent cost, location, max occupants |
| `chat` | Direct messages between pairs of users (`user_id1`, `user_id2`) |
| `request` | Room/join requests with `content` (`'Join'` or `'Rent'`), `id1` (requester), `id2` (owner), `status` (0=pending, 1=accepted, 2=rejected), `room_id` |
| `party` | User groups for joint room-seeking |
| `accessory` / `have_accessory` | Room amenities (air con, internet, key card, fridge, etc.) and their room assignments |
| `picture_room` | Room photo paths relative to `../view/img/up/` |
| `interest` / `who_interest` | User interests (free-text in `who_interest`, enumerated in `interest`) |
| `education` / `hstory_education` | School/university history per user |
| `religion`, `city`, `area`, `country`, `jobs` | Reference/lookup data |

### User types

- **Room seekers**: `haveroom = 0`, `user_name = '1'` (email login) or `user_name = '2'` (Facebook OAuth, has `fbid`)
- **Room owners**: `haveroom = 1`
- Roommate-compatibility fields on `users`: `clean_lvl`, `snoring`, `smoke`, `children`, `party_lvl` (all integer 1–5 scales)
- `rent_cost` and `period` on `users` represent the seeker's budget and preferred rental period

### Character encoding

Mixed charsets throughout: `tis620_bin` (Thai-specific columns), `utf8`/`utf8mb4` (name/description fields), `latin1` (numeric/reference columns). When modifying schema, match the existing charset of sibling columns to avoid collation conflicts.

## Test Accounts

| Role | Username | Password |
|---|---|---|
| Room owner | `findmrm01@gmail.com` | `findmrm01findmrm01` |
| Room seeker | `findmrm03@gmail.com` | `findmrm03findmrm03` |
