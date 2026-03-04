# AI Pillbox 🧪💊


A lightweight **web-based medication management prototype** with separate **Patient** and **Doctor** portals, backed by **Google Sheets + Apps Script**.  
The project is also designed as an **X / Y / Z experiment platform** to study how different digital interventions affect **medication adherence**.

> 🔬 This is a prototype for research / coursework, **not** a medical device and **not** for real clinical use.

---

## 1. Features

### 🌐 Two Web Portals

#### Patient Portal (`patient.html`)
- Record **patient name**
- Record **medication name**
- Set **suggested daily intake time** (e.g. `08:00`)
- Select **study group**: `X`, `Y`, or `Z`
- Save records to:
  - Local browser storage (`localStorage`)
  - Cloud database (Google Sheet via Apps Script)
- See a simple **medication history table** for the current patient

#### Doctor Portal (`doctor.html`)
- Load all records from **Google Sheet (`pillbox` sheet)**
- Show **summary statistics**:
  - Total records
  - Total patients
  - Records per group (X vs Y/Z)
- Select a patient to:
  - View their full **medication timeline**
  - See **group label** (X / Y / Z)
  - Get a **real-time suggestion** based on the latest `suggestedTime` and current time

---

## 2. Research Scenario: X / Y / Z Groups

This project is designed to support a simple experimental design:

- **Group X – Intelligent Intervention**
  - Full AI Pillbox usage (patient logging + doctor monitoring + real-time suggestion)
- **Group Y – Basic Reminder**
  - Basic time reminders only (e.g. alarms), no intelligent / doctor-side monitoring
- **Group Z – Control / Usual Care**
  - No digital tool; only standard oral / paper instructions

### Core Outcome

- **Medication adherence rate** per patient, over the observation period, e.g.:

\[
\text{Medication adherence} = \frac{\text{Number of doses taken (on time)}}{\text{Number of doses prescribed}} \times 100\%
\]

The data logged by AI Pillbox (especially group X) can be used later for:
- ANOVA / t-tests comparing X vs Y vs Z  
- Visualizations of adherence distribution across groups

---

## 3. Tech Stack

### Frontend
- **HTML5 + CSS3 + Vanilla JavaScript**
- **Axios** for HTTP requests to Apps Script
- Responsive layout (simple CSS, no framework)
- Deployed as a static site (e.g. **Netlify**)

### Backend (Serverless)
- **Google Apps Script** providing an HTTP API:
  - `doGet(e)` with `action` parameter:
    - `action=insert_pillbox` → insert a new record into sheet `pillbox`
    - `action=read&table=pillbox` → read all rows from sheet `pillbox`

### Data Store
- **Google Sheets** acting as a lightweight cloud database:
  - Spreadsheet ID configured in Apps Script
  - Sheet name: `pillbox`

---

## 4. Data Model

Google Sheet `pillbox` has the following columns (header row):

| Column           | Description                                        |
|------------------|----------------------------------------------------|
| `timestamp`      | Server insert time (`new Date()` in Apps Script)   |
| `patientName`    | Patient identifier / nickname                      |
| `group`          | Study group: `X`, `Y`, or `Z`                      |
| `medication`     | Medication name (e.g. “aspirin 100mg”)             |
| `suggestedTime`  | Suggested daily intake time (e.g. `08:00`)        |
| `recordDateTime` | Client-side record time (from patient portal)      |

---

## 5. Project Structure

Example folder structure for the front-end:

```text
.
├─ index.html        # Landing page + links to Patient/Doctor portals
├─ patient.html      # Patient Portal (records + send to Sheet)
├─ doctor.html       # Doctor Portal (read from Sheet, show stats)
└─ README.md         # This file

