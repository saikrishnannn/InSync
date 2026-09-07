# InSync 🌸

> **Understand her cycle. Stay in sync.**  
> A privacy-first, dual-role menstrual cycle tracker and partner-support web app designed to bridge biological data with proactive empathy.

Live Demo: [https://insync-0.web.app/](https://insync-0.web.app/)

---

## 📌 Overview

Most cycle tracking apps treat reproductive health as an isolated, clinical silo. They overwhelm users with medical jargon, graphs, and paywalls—leaving partners in the dark and placing the emotional burden on women to explain their physical fatigue, pain, and mood shifts.

**InSync** reimagines cycle tracking as a shared, relational experience:
- **For Her:** A zero-friction cycle, lifestyle, and fitness hub featuring cycle-synced workouts, nutrition buffers, and predictive supply stash alerts.
- **For Him:** The **Partner Translation Engine**, which ingests her phase and logged comfort levels, stripping away the guesswork to deliver actionable guidance across communication, date planning, food, and daily gestures.

---

## ✨ Key Features

### 🔄 The Partner Translation Engine
Translates clinical phase data (e.g., *Late Luteal Phase*) into 4 actionable buckets:
- **Communication Tone:** How to communicate with empathy and reduce conversational friction.
- **Date Planning:** Low-energy vs. high-energy social activity suggestions.
- **Nutrition & Cravings:** Phase-specific comfort meals and micronutrient suggestions.
- **Proactive Gestures:** Small, high-impact actions to support her day.

### ✈️ In-Person vs. Long-Distance Mode
An instant, client-side toggle that dynamically alters partner advice:
- **In-Person:** Focuses on physical care (heating pads, home-cooked meals, physical presence).
- **Long-Distance:** Shifts to remote gestures (surprise deliveries, digital care packages, low-pressure check-ins).

### 🔒 Privacy-by-Design Architecture
- **Zero-PII Onboarding:** Uses Firebase Anonymous Auth. No email, password, or phone number required.
- **6-Character Pairing Handshake:** Devices connect securely using a single-use pairing code.
- **Client-Side Data Isolation:** Sensitive journal notes, symptom logs, and BBT remain sandboxed in browser storage (`localStorage` / `IndexedDB`) on the user's physical device and never touch a network packet.
- **Sanitized Real-Time Sync:** Only high-level, non-identifiable status metadata (current phase, comfort score, low-supply alerts) syncs to Firestore via real-time listeners.

### 📦 Smart Stash & Supply Depletion Predictor
- Tracks physical pad/tampon inventory.
- Decrements counts automatically on logged bleed days.
- Triggers low-stock alerts 2 days prior to the predicted cycle onset to prevent emergency store runs.

### 🏋️ Cycle-Synced Fitness & Macros
- Adapts training recommendations by phase (progressive overload in follicular; deloads and active recovery in luteal).
- Suggests automatic caloric buffers (+100 to +250 kcal) to support luteal metabolic demands.

### ⚡ Batch-Logging & Cycle Health History
- **One-Tap Predictive Logging:** Auto-populates expected period duration to eliminate daily check-in burnout while preserving day-by-day editability.
- **Cycle Health Analytics:** Retrospective view tracking historical cycle lengths, period duration, and ovulation milestones over time.

---

## 🛠️ Tech Stack

| Layer | Technology |
| :--- | :--- |
| **Frontend** | React (JSX), Tailwind CSS |
| **State & Local Storage** | React Hooks, Web Storage API (`localStorage` / `IndexedDB`) |
| **Backend & Sync** | Firebase Firestore (Real-time listeners & Security Rules) |
| **Authentication** | Firebase Anonymous Auth |
| **Hosting** | Firebase Hosting |

---

## 🧠 Architectural & Algorithmic Highlights

- **Modified Calendar Projection (Ogino-Knaus):** Dynamically computes phase intervals (Menstrual, Follicular, Ovulation/Fertile Window, and Luteal) and continually recalibrates predicted start dates as historical cycles are recorded.
- **Deterministic Tip Rotation:** Evaluates date seeds, phase keys, and distance modes through a client-side hashing algorithm, rotating across 160+ curated advice cards synchronously between partners without incurring Firestore read costs.
- **Selective Data Firewall:** Implements an intentional sync boundary ensuring raw client logs remain purely local while sending only sanitized state updates across the wire.

---

## 🚀 Getting Started

### Prerequisites
- Node.js (v18+ recommended)
- npm or yarn
- A Firebase project with Firestore and Anonymous Auth enabled

### Installation

1. **Clone the repository:**
   ```bash
   git clone [https://github.com/your-username/insync.git](https://github.com/your-username/insync.git)
   cd insync
