AI-Powered Hospital Appointment Scheduling with VAPI & n8n
Voice assistant (“Ken”) that books, reschedules, and cancels hospital appointments using VAPI for calls and n8n for workflow automation with Google Sheets as the source of truth.


✨ What this does
Human-like voice booking over phone

Real-time slot lookup from Google Sheets

Appointment booking directly from call

Reschedule and cancellation flows

Multi-doctor, multi-location friendly

Clear, patient-first conversation design

🗺️ Architecture
scss
Copy
Edit
Caller → VAPI (Ken) → Tools → n8n Workflows → Google Sheets
Add your images under /docs/ and reference them:
![Flow](docs/vapi_n8n_flowchart.png)
![High-Level](docs/vapi_n8n_high_level_architecture.png)
![Compact](docs/vapi_n8n_compact_architecture.png)

🧩 Tech Stack
VAPI – Voice AI agent for natural conversations

n8n – Orchestration (slot lookup, booking)

Google Sheets – Schedules, slots, appointments

🗣️ Assistant Behavior (Prompt Summary)
Greets, understands intent, collects: reason, preferred date/time, doctor

Uses Free_slot_info_tool to fetch options

Confirms a slot and calls appointment_booking

Repeats details, shares prep info, closes warmly

🛠️ Tools (VAPI)
Free_slot_info_tool
Use when: Appointment type + date/time preference (+ doctor if any) are known

Returns: List of available slots { date, day, start, end, doctor }

appointment_booking
Use when: Caller confirms specific slot

Returns: Booking confirmation { appointment_id, date, time, doctor }

Implement these tools as VAPI “functions” that call your n8n HTTP endpoints.

⚙️ Setup
1) Prerequisites
VAPI account & phone number (or SIP/WebRTC)

n8n (cloud or self-hosted, ≥ v1.0)

Google Sheet with tabs:

Doctors(id, name, department, location)

Slots(id, doctor_id, date, start_time, end_time, status)

Appointments(id, patient_name, phone, doctor_id, date, start_time, end_time, status)

2) n8n environment
Create n8n/.env (or use your deployment method):

bash
Copy
Edit
N8N_HOST=https://your-n8n.example.com
GOOGLE_SHEETS_CREDENTIALS_JSON='{"type":"service_account","project_id":"..."}'
SHEETS_SPREADSHEET_ID=YOUR_SPREADSHEET_ID
3) Import workflows
Import JSON from n8n/workflows/:

slot_lookup.json

appointment_booking.json

Publish and note each workflow’s public endpoint URL.

4) Configure VAPI
Create assistant “Ken”.

Paste vapi/assistant_prompt.md.

Add tools with function signatures that POST to your n8n endpoints:

Free_slot_info_tool → POST /slot-lookup

appointment_booking → POST /book

Example tool payloads:

json
Copy
Edit
// Free_slot_info_tool input
{
  "appointment_type": "General Checkup",
  "preferred_date": "2025-08-12",
  "preferred_time_window": "09:00-13:00",
  "preferred_doctor": "Dr. Smith"
}
json
Copy
Edit
// appointment_booking input
{
  "patient_name": "Jane Doe",
  "phone": "+91XXXXXXXXXX",
  "doctor_id": "DOC_001",
  "slot_id": "SLOT_123",
  "status": "Booked"
}
▶️ How it Works (Call Flow)
Caller explains need → Ken gathers reason, date/time, doctor

Ken calls Free_slot_info_tool → gets options from n8n (Sheets)

Ken offers 2–3 best slots in friendly language

Caller chooses → Ken calls appointment_booking

n8n writes to Sheets

Ken confirms details and wraps up

🧪 Demo
Screen recording only (no faces)

Show:

A call where the caller requests a slot

Ken checks availability → offers options

Booking confirmation

Row added to Appointments sheet

Place the video link here once ready.

✅ Submission Checklist
 Source code / workflow design (n8n exports, VAPI prompt & tools)

 Demo video (screen recording)

 Short documentation (Problem → Solution → Tech Stack)

🔐 Notes on Privacy & Safety
Do not store PHI beyond what’s necessary for scheduling

Use service accounts for Sheets access

Log workflow events without sensitive content

🤝 Contributing
PRs are welcome! Please open an issue for feature requests or bugs.

📄 License
This project is licensed under the MIT License. See LICENSE for details.
