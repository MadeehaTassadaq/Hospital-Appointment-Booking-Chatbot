# 🏥 Appointment Booking Chatbot (n8n Workflow)

## 📋 Overview
This project automates appointment booking and hospital inquiry workflows using **n8n**, **OpenAI**, **Google Gemini**, **Google Sheets**, and **Gmail**.  
It was designed for **New Mowasat Hospital** to allow users to:
- Chat naturally about hospital services and departments.  
- Check doctor availability.  
- Book appointments automatically via an AI-powered chatbot.  
- Store booking details in Google Sheets.  
- Send confirmation emails automatically.

---

## ⚙️ Features
✅ **AI-Powered Chatbot** – Handles three main intents:
1. **General Chat** – Answers questions about hospital services.  
2. **Doctor Lookup** – Retrieves availability from Google Sheets.  
3. **Appointment Booking** – Collects and processes booking details.

✅ **Google Sheets Integration** –  
Doctor availability and patient appointment data are read/written to connected Google Sheets.

✅ **Automated Email Confirmation** –  
Once a booking is confirmed, an automated Gmail message is sent to the patient.

✅ **Memory System (PostgreSQL)** –  
Maintains conversation context between user messages.

✅ **Webhook Endpoint** –  
Integrates easily with a front-end chatbot or web app.

---

## 🧩 Workflow Structure

| Node | Purpose |
|------|----------|
| **Webhook** | Receives incoming user messages from frontend or API. |
| **AI Agent (Main Chat Agent)** | Detects user intent (`chat`, `doctor_lookup`, or `booking_request`) and generates structured output JSON. |
| **Structured Output Parser** | Ensures AI output follows the defined JSON schema. |
| **Code in JavaScript** | Cleans and parses AI response into valid JSON. |
| **If Node** | Routes logic — if `type == booking_request`, triggers booking workflow. |
| **AI Agent1 (Action Agent)** | Takes structured booking data and triggers actions (Google Sheets + Gmail). |
| **Add Appointment Information (Google Sheets)** | Appends new row with appointment details. |
| **Send a message in Gmail** | Sends confirmation email to patient. |
| **Respond to Webhook** | Returns AI response back to frontend. |

---

## 🧠 AI Model Configuration

### 🩺 Main AI Agent
**System Prompt:**
> You are an intelligent hospital assistant AI for New Mowasat Hospital.  
> Handle three main tasks: general chat, doctor lookup, and appointment booking.  
> Always output valid JSON in the schema:
> ```json
> {
>   "type": "chat" | "doctor_lookup" | "booking_request",
>   "message": "string",
>   "data": {
>     "Doctor Name": "",
>     "Date": "",
>     "Time": "",
>     "Patient Name": "",
>     "Email": "",
>     "Phone": "",
>     "Status": ""
>   }
> }
> ```

Connected Tools:
- **Google Sheets** (Doctor Schedule)
- **Postgres Chat Memory**
- **Structured Output Parser**
- **OpenAI GPT-4 / Gemini Chat Model**

---

### 🤖 Action Agent
**System Prompt:**
> You are the Mowasat Hospital Action Agent.  
> Process confirmed appointment JSONs.  
> Append data to Google Sheet and send email confirmation using connected tools.  
> Return a polite success message.

Connected Tools:
- **Google Sheets (Appointment Data)**
- **Gmail**
- **Postgres Memory**
- **OpenAI GPT-4.1-mini**

---

## 📊 Google Sheets Setup

**Doctor Schedule Sheet:**
- Columns: `Doctor Name | Department | Available Days | Timings`

**Patient Booking Data Sheet:**
- Columns:  
  `Patient Name | Doctor Name | Date | Time | Email | Phone | Status`

---

## 📧 Gmail Automation

- **Tool Name:** `Send a message in Gmail`
- **Subject:** `"Hello your appointment is booked"`
- **Body Example:**
  ```
  Hello {{Patient Name}}!
  Your appointment with {{Doctor Name}} is booked for {{Date}} at {{Time}}.
  Please arrive on time.
  Thank you for trusting Mowasat Hospital.
  ```

---

## 🚀 Deployment & Usage

### 1. Import Workflow
1. Open your [n8n.cloud](https://app.n8n.cloud) or local n8n instance.
2. Go to **Workflows → Import from file**.
3. Upload `My workflow 2.json`.

### 2. Configure Credentials
Make sure the following credentials are connected:
- `OpenAI API`
- `Google Sheets OAuth2`
- `Gmail OAuth2`
- `Postgres`

### 3. Start the Workflow
- Activate your workflow in n8n.
- Send a POST request to your Webhook URL:
  ```
  POST https://<your-n8n-domain>/webhook/<webhook-path>
  {
    "message": "I want to book an appointment with Dr. Hamza Malik tomorrow at 10 am"
  }
  ```

### 4. Verify Output
- Check your **Google Sheet** for the new booking entry.
- Confirm receipt of **email** in the patient’s inbox.

---

## 🧱 Tech Stack

| Component | Technology |
|------------|-------------|
| AI Model | OpenAI GPT-4 / Gemini Chat |
| Workflow Automation | n8n |
| Database | PostgreSQL |
| Email Service | Gmail |
| Data Storage | Google Sheets |

---

## 📁 Repository Structure

```
📦 appointment-booking-chatbot
 ┣ 📄 README.md
 ┣ 📄 My workflow 2.json
 ┗ 📂 assets/
     ┗ screenshots or docs
```

---

## 🧩 Example JSON Response

```json
{
  "type": "booking_request",
  "message": "Thank you! Your appointment with Dr. Ayesha Khan for tomorrow at 2:00 PM is confirmed.",
  "data": {
    "Doctor Name": "Dr. Ayesha Khan",
    "Date": "Tomorrow",
    "Time": "2:00 PM",
    "Patient Name": "Madeeha Tassadaq",
    "Email": "drmadeehatassadaq@gmail.com",
    "Phone": "032465678",
    "Status": "Pending"
  }
}
```

---

## 🧑‍💻 Author
**Madeeha Tassadaq**  
Project: *AI Appointment Booking Chatbot*  
Institution: *New Mowasat Hospital*  
Built with ❤️ using **n8n** + **OpenAI** + **Google APIs**
