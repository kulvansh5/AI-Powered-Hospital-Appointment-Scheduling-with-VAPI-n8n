# **AI-Powered Hospital Appointment Scheduling with VAPI & n8n**

> A smart voice assistant that automates hospital appointment booking, rescheduling, and cancellation using **VAPI** for real-time calls and **n8n** for workflow automation with **Google Sheets** as the central database.

---

## 📌 **Overview**
This project implements a voice-based hospital appointment scheduling system that allows patients to **speak naturally** with an AI agent (**Ken**). The assistant can:
- Check **real-time slot availability** from Google Sheets
- Book appointments after confirmation
- Offer rescheduling and cancellation
- Support multiple doctors and locations

The result is **faster bookings**, **fewer errors**, and **better patient experience** without increasing front-desk workload.

---

## 🗺 **System Architecture**
```
Caller → VAPI Voice Assistant (Ken) → Tools → n8n Workflows → Google Sheets
```

---

## 🧠 **How It Works**
1. **Greeting & Intent Detection**  
   Ken greets the patient and identifies whether they want to book, reschedule, or cancel an appointment.
2. **Information Gathering**  
   Ken collects the appointment reason, preferred date/time, and doctor.
3. **Slot Lookup**  
   The `Free_slot_info_tool` checks Google Sheets for available slots.
4. **Confirmation & Booking**  
   Once the patient selects a slot, `appointment_booking` is triggered to reserve it.
5. **Wrap-Up**  
   Ken confirms the booking details and ends the call warmly.

---

## 🛠 **Tools (VAPI Functions)**

### `Free_slot_info_tool`
- **Purpose:** Retrieve available appointment slots.
- **When to Use:** After collecting appointment type, preferred date/time, and doctor.
- **Returns:** List of available slots `{ date, day, start, end, doctor }`.

### `appointment_booking`
- **Purpose:** Confirm and lock in an appointment.
- **When to Use:** After patient confirms a specific slot.
- **Returns:** Booking confirmation `{ appointment_id, date, time, doctor }`.

---

## 🧩 **Tech Stack**
- **VAPI** – Voice AI agent for handling patient calls  
- **n8n** – Workflow automation and tool orchestration  
- **Google Sheets** – Centralized storage for doctors, slots, and appointments  

---

## ⚙ **Setup Guide**

### **1. Prerequisites**
- VAPI account & number (SIP/WebRTC)
- n8n (cloud or self-hosted, v1.0+)
- Google Sheet with:
  - `Doctors(id, name, department, location)`
  - `Slots(id, doctor_id, date, start_time, end_time, status)`
  - `Appointments(id, patient_name, phone, doctor_id, date, start_time, end_time, status)`

### **2. Configure n8n**
Create `.env` file:
```bash
N8N_HOST=https://your-n8n.example.com
GOOGLE_SHEETS_CREDENTIALS_JSON='{"type":"service_account","project_id":"..."}'
SHEETS_SPREADSHEET_ID=YOUR_SPREADSHEET_ID
```
Import workflows:
- `slot_lookup.json`
- `appointment_booking.json`

Publish workflows and note their endpoint URLs.

### **3. Configure VAPI**
- Create assistant “Ken”.
- Paste `assistant_prompt.md`.
- Add tools:
  - `Free_slot_info_tool` → POST to `slot_lookup` workflow
  - `appointment_booking` → POST to `appointment_booking` workflow

---

## 📞 **Example Interaction**
**Caller:** “Hi, I’d like to book an appointment with Dr. Smith next week.”  
**Ken:** “Sure! Let me check… I have Tuesday at 2:30 PM or Thursday at 10:00 AM. Which works best?”  
**Caller:** “Tuesday, 2:30.”  
**Ken:** “Perfect. I’ve booked you for Tuesday, January 15th at 2:30 PM with Dr. Smith.”  

---

## 📽 **Demo**
*(Add your video link here once available)*

---

## 🔐 **Privacy & Security**
- No unnecessary PHI storage  
- Service account for Sheets access  
- Minimal workflow logging (no sensitive details)  

---

## 📄 **License**
This project is licensed under the **MIT License**. See [LICENSE](LICENSE) for details.
