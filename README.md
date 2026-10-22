# Real Estate Management — n8n Automation

An AI-powered real estate WhatsApp assistant built with n8n, Google Gemini, WhatsApp, and Google Sheets to answer property enquiries, manage site visits, handle FAQs, and record complaints.

## 1. Overview

This workflow helps customers use WhatsApp to find available properties, ask questions, book or manage site visits, and register complaints or callback requests. Property and FAQ answers are sourced from Google Sheets.

- Supports English, Hindi, and Hinglish replies.
- Shows up to three matching available properties per response.
- Books a visit only after customer confirmation and a successful tool operation.
- Tracks site visits and complaints in Google Sheets.
- Uses short conversation memory keyed by the customer's WhatsApp number.

## 2. Demo Video

[Watch the Real Estate Management Demo](https://youtu.be/MCaHumjmY0Q?si=14aWk0-fsDMai-B-)

## 3. Tech Stack & Tools

- n8n — workflow automation
- WhatsApp Business Platform — customer messaging
- Google Gemini — AI responses
- Google Sheets — properties, FAQs, visits, and complaints

## 4. Prerequisites

- n8n workspace
- WhatsApp Business API setup, access token, and phone number ID
- Google Gemini API access
- Google Sheets access with the required tabs
- The workflow JSON in `workflows/real-estate-management.json`

## 5. Setup / Import Workflow

1. Download `workflows/real-estate-management.json`.
2. In n8n, choose **Import from File** and select the JSON.
3. Reconnect WhatsApp, Google Sheets, and Google Gemini credentials.
4. Create or select a spreadsheet with the tabs and columns listed below.
5. Replace `YOUR_GOOGLE_SHEET_ID` and `YOUR_PHONE_NUMBER_ID` with your own values in the imported workflow.
6. Configure the WhatsApp access token securely; do not commit a real token.
7. Test each scenario before activating the workflow.

## 6. Configuration Details

The spreadsheet uses four tabs:

| Tab | Purpose |
|---|---|
| `Properties` | Available property records |
| `FAQs` | Approved customer FAQ answers |
| `SiteVisits` | Visit bookings and status |
| `Complaints` | Complaints and callback requests |

See `database/` for CSV templates. The provided templates omit real customer visit and complaint records.

Visit rules described in the workflow: Monday–Saturday, 10:00 AM–6:00 PM; no Sundays. The customer must confirm the property, date, and time before booking.

## 7. How to Run

1. Configure the required credentials and spreadsheet ID.
2. Confirm the sheet names and column names match the workflow.
3. Activate the workflow and send a test message to the connected WhatsApp number.
4. Test property search, FAQ responses, visit booking/rescheduling/cancellation, and complaint registration/status.
5. Check the corresponding Google Sheets tab after each write operation.

## 8. Sample Input / Output

**Example input:** “I’m looking for a 2BHK in Shela, Ahmedabad. What options are available?”

**Expected behavior:** The assistant checks the Properties sheet, replies only with available matching records (up to three), and does not invent prices or features. Actual output depends on the spreadsheet data.

**Site visit example:** The assistant collects the required details, checks availability, asks the customer to confirm, and only reports success after the booking tool succeeds.

The accompanying documentation does not include recorded test results; run the scenarios before production use.

## 9. Documentation

- [Workflow JSON](workflows/real-estate-management.json)
- [Detailed workflow documentation](DOCUMENTATION.md)
- [CSV templates](database/)

## 10. Important Safety Notes

- The workflow file has been sanitized: credentials, the original spreadsheet ID, and the WhatsApp token have been removed or replaced with placeholders.
- Reconnect credentials after import and store tokens in n8n credentials or another secure secret store.
- The workflow's rescheduling tool description and sheet field names should be checked for consistency.
- Slot checking is not a transactional lock; concurrent bookings may conflict.
- Staff alerts are not included in the documented workflow. Restrict access to sheets containing personal information.

## 11. Footer

**Built by Optimity Logics**

#OptimityLogics #RealEstateAutomation #n8n #WhatsAppAutomation #AIAutomation
