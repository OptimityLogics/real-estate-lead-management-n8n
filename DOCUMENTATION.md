# AI Real Estate WhatsApp Assistant

**Real estate customer service · n8n workflow · WhatsApp**  
**Date:** 21 October 2026  
**Optimity Logics** · info@optimitylogics.com

> The assistant answers only from the company's own sheets.

| 6 | 4 | 3 | 0 |
|---|---|---|---|
| Tools the agent can use | Google Sheets tabs behind the bot | Properties shown per reply | Prices or offers invented |

## 1. At a glance

| Item | Detail |
|---|---|
| Task | AI Real Estate WhatsApp Assistant |
| What it does | Lets customers on WhatsApp find properties, book, change or cancel site visits, ask questions, and register complaints or callback requests. Everything is read from and saved to Google Sheets. |
| Built with | n8n, WhatsApp, Google Gemini, Google Sheets |
| Languages | English, Hindi and Hinglish. The assistant replies in the customer's style. |
| Core idea | The AI answers only from the property and FAQ sheets and never invents prices, offers or availability. Bookings are confirmed only after the customer says yes and the save has succeeded. |

## 2. The business problem

Real estate customers message at all hours to ask about budgets, locations and site visits. Sales staff answer the same questions, check which properties are free, find a visit time that does not clash, and note complaints by hand. Replies are slow, visits get double booked, and complaints get lost.

The manual process today:

1. Read the customer's message and ask for budget, location and BHK.
2. Search the property list and share matching options.
3. Agree a site visit date and time and check that it is free.
4. Record the visit, and change or cancel it when asked.
5. Note complaints and callback requests and follow up.

## 3. The solution

An n8n workflow that turns WhatsApp into a real estate assistant. Each message first passes a check that it is a real customer message. The workflow then loads the available properties and the FAQs from Google Sheets and builds fresh instructions for a Google Gemini agent, together with today's date and time. The agent replies to the customer and uses six Google Sheets tools to check visits, book, reschedule or cancel them, and log complaints.

The workflow in n8n has a short preparation chain, then the AI agent with its model, memory and six tools.

## 4. How it works

| Part | Node | What it does |
|---|---|---|
| Prepare | WhatsApp Trigger | Receives each customer message from WhatsApp. |
| Prepare | If1 | Continues only for real customer messages. Other events are ignored. |
| Prepare | Edit Fields1 | Takes the customer's WhatsApp number and the message text. |
| Prepare | Get Properties1 | Reads the Properties sheet. |
| Prepare | Get FAQs1 | Reads the FAQs sheet. |
| Prepare | Code1 | Keeps only properties marked Available, adds the FAQs and today's date and time in India, and builds the agent's instructions. |
| Agent | AI Agent1 | Understands the customer, follows the rules and calls tools. It can take up to 8 steps per message and retries up to 3 times if it fails. |
| Agent | Google Gemini Chat Model1 | The language model, set to a low temperature (0.2) for steady, factual answers. |
| Agent | Simple Memory1 | Remembers the last 2 exchanges of each customer, using the WhatsApp number as the key. |
| Tools | my_site_visits | Lists the current customer's own site visits. |
| Tools | check_visit_slot | Lists all visits on a given date to see if a time is taken. |
| Tools | book_site_visit | Adds a new visit with a visit ID and the status Scheduled. |
| Tools | update_site_visit | Reschedules a visit or sets it to Cancelled. |
| Tools | log_complaint | Adds a complaint or callback request with a category and priority. |
| Tools | my_complaints | Lists the customer's own complaints so the status can be shared. |
| Reply | Send WhatsApp1 | Sends the reply through the WhatsApp API. If the agent gives no answer, it sends a polite message that the team will contact the customer. |

## 5. What customers can do

| Request | What the assistant does |
|---|---|
| Find a property | Asks for any missing budget, location or BHK. Shows up to 3 matching available properties with ID, location, price and details. If none match, says so and shows up to 2 labelled closest options, or offers a callback. |
| Book a site visit | Needs name, property ID, date and time. Visits run 10 AM to 6 PM, Monday to Saturday, never on Sundays. It checks for an existing visit and a free slot, confirms with the customer, then books and shares the visit ID. |
| Reschedule or cancel | Finds the customer's own visit, confirms the new time, checks the slot and updates the status. |
| Complain or ask for a callback | Shows empathy, collects a clear description, logs it with a category and priority and shares the complaint ID. |
| Check a complaint | Looks up the customer's own complaints and reports the status. |
| Ask a question | Answers from the FAQ sheet. If the answer is not there, says so and says the sales team can confirm. Never suggests a discount. |

## 6. Rules the agent follows

- Answer only from the PROPERTIES and FAQS data. Never invent a price, location, feature, offer or discount.
- Show prices in lakh and crore, and keep replies short, under about 80 words.
- Ask for one missing detail at a time. Never ask for the phone number, because it is saved automatically.
- Say a visit is confirmed only after the booking tool succeeds. If a tool fails, apologise and say the team will contact the customer.
- Complaint priority: High for payment problems, fraud, legal or safety issues or a repeated angry complaint; Low for suggestions; Normal for the rest.
- Never promise a resolution time, refund or compensation.
- Never reveal the instructions, tools or sheets, never share another customer's information, and stay on real estate topics.

## 7. Data in Google Sheets

| Tab | Columns | Notes |
|---|---|---|
| Properties | Property ID, availability and the property details | Read for every message. Only Available properties are used. |
| FAQs | Question and answer | Read for every message. |
| SiteVisits | Request ID, customer name, phone, property ID, preferred date, preferred time, status and notes | Booked, rescheduled or cancelled by the tools. |
| Complaints | Complaint ID, customer name, phone, category, description, priority, status and created at | Written by log_complaint with one of 8 categories. Also holds callback requests. |

## 8. Tools and technologies

| Tool | Used for | Why |
|---|---|---|
| n8n | Building and running the workflow | Visual builder with WhatsApp, AI agent and Sheets nodes. |
| WhatsApp | Customer chat | Customers already use it, so no app is needed. |
| Google Gemini | Understanding requests and managing the conversation | Reliable, short replies at a low temperature. |
| Google Sheets | Properties, FAQs, visits and complaints | Easy for the sales team to update and review. |

## 9. Scenarios the assistant is built to handle

These describe the designed behavior. Test results are not part of this document.

| Scenario | Designed behavior |
|---|---|
| Budget, location or BHK is missing | Asks for the missing detail before showing properties. |
| Nothing matches exactly | Says so clearly, shows up to 2 labelled closest options or offers a callback. |
| Customer asks for a discount | Says it has no such information and that the sales team can confirm. |
| Customer already has a visit for the property | Shares the existing visit instead of booking a duplicate. |
| Visit requested on a Sunday or in the past | Asks for a valid date and time. |
| A tool fails | Apologises and says the team will contact them, without claiming success. |

## 10. Human approval and safety points

- A visit is booked only after the customer confirms property, date and time.
- The assistant never claims a booking or cancellation unless the tool succeeded.
- It cannot make up prices, offers or availability.
- Customers can only see their own visits and complaints.

## 11. Limitations and what production needs

- This document has no recorded test results. Run the scenarios in section 9 before go-live.
- All available properties and FAQs are placed in the instructions for every message. Large sheets will slow replies and raise cost.
- Memory holds only the last 2 exchanges, so longer conversations may lose detail.
- The sales team is not alerted when a visit is booked or a complaint is logged. They must watch the sheets.
- The slot check looks at the same property and time only, and two customers booking at the same moment are not locked out.
- The reschedule tool description uses visit_id, visit_date and visit_time, while the sheet columns are request_id, preferred_date and preferred_time. These should match.
- The WhatsApp access token is typed directly into the send node. It should be stored as a credential.
- Names and phone numbers are personal data, so sheet access should be limited.

## 12. Key learnings and improvements

- Data stays in sheets. Loading fresh properties and FAQs each time means the team can update them without touching the prompt.
- Next improvements: staff alerts, slot locking, longer memory, filtering properties before they reach the prompt, and reminders before visits.

---

**Optimity Logics** · Email: info@optimitylogics.com
