# CampusConnect+ 🎓

**🌐 Live Project Link:** https://campusconnectplus.netlify.app/
**💻 GitHub Repository:** https://github.com/AaryanOdedra01/IDAI101-2305026--AaryanOdedra.git

---

Welcome to my submission for the **Creating Intelligent Chatbots for Practical Applications** assignment. I chose to tackle Scenario 2: ParentConnect AI, and built a virtual school communication assistant named **CampusConnect+**. 

---

## 📌 What is this thing?

CampusConnect+ acts as a bridge between parents and the school's administrative office. It handles the repetitive, everyday questions—like checking attendance, exam schedules, fee deadlines, and circulars. 

The goal here is simple: give parents instant answers without making them wait on hold or for an email reply, while freeing up school staff to handle more complex issues. The conversational brain is built using **Google Dialogflow ES**, relying entirely on its built-in Natural Language Understanding (NLU) and Custom Payloads.

---

## 🖥️ The Frontend Website

To make this project feel like a real-world application, I didn't just leave the chatbot in the Dialogflow testing console. I built a complete, responsive frontend website to host it.

* **Modern UI:** Designed with a vibrant animated gradient background and "glassmorphism" (frosted glass) UI cards to make it feel welcoming, professional, and engaging.
* **Single-File Architecture:** The entire site runs on pure HTML, CSS, and JavaScript. No heavy frameworks or complex backend servers required.
* **Integrated Mock Portals:** When the chatbot gives a parent a link to "download an admit card" or "pay fees," those links actually work! They trigger sleek pop-up modals right on the page that simulate:
    * Admit Cards
    * Fee Receipts
    * Term Report Cards
    * A Secure Fee Payment Portal
* **Print-Ready Documents:** The mock document portals feature custom CSS. If a user clicks "Print Document," the website hides the colorful background and only prints the clean, white document, just like a real PDF download.

---

## 🧠 How the AI works (the short version)

When a parent asks a question—whether they type *"attendance for grade 8"* or casually mix in Hindi phrasing like *"kab hai exams"*—Dialogflow maps it to the correct **intent**. It extracts specific details (**entities**) like the grade level or subject, and uses slot-filling to politely ask for any missing information before giving an answer.

My main focus while building this was making the chat feel fluid and connected, rather than like a rigid Q&A form. If a parent asks about the exam schedule and follows up with *"when will results come out?"*, the bot remembers the grade level they were just talking about. I achieved this using Dialogflow's **contexts**, which pass information seamlessly from one intent to the next.

---

## 🛠️ Everything I built

I went beyond the required 15 intents and 10 entities to make sure the conversational flows actually felt complete and didn't leave the user stranded. 

### Entities (11 custom + 4 built-in)

| Entity | What it captures |
|---|---|
| `@grade-level` | Grade/Class/Std from Nursery to 12, including phrasing variants parents actually use |
| `@subject` | Math, Science, English, etc. |
| `@exam-type` | Unit test, mid-term, final, pre-board, surprise test |
| `@fee-type` | Tuition, transport, exam, admission, hostel, activity, library, lab |
| `@announcement-type` | Holiday, event, circular, meeting, PTA |
| `@day-of-the-week` | Monday through Sunday |
| `@contact-type` | Class teacher, principal, admin office, transport dept, accounts dept, counselor |
| `@document-type` | Report card, admit card, fee receipt, ID card, TC, bonafide certificate |
| `@payment-mode` | Online, cash, cheque, UPI, card |
| `@absense-reason` | Sick, family emergency, travel, medical appointment, etc. |
| `@student-id` | A regex entity — matches any 7-digit student ID (`^\d{7}$`) |
| **Built-in** | `@sys.date`, `@sys.date-period`, `@sys.person`, `@sys.boolean` |

### Intents (33 total)

Here is how the conversational paths break down:

*   **Attendance:** `attendencecheckintent`, `attendancedetailedreportintent`, `attendanceminimumintent`, `absensereportintent`, `absenceconfirmintent`, `absenceeditintent`
*   **Exams:** `examscheduleintent2`, `examresultintent`, `admitcardintent`, `setreminderintent`
*   **Fees:** `feedetailsintent`, `feeduedateintent`, `feereceiptintent`, `paymentredirectintent`, `paymenthistoryintent`, `feeextensionintent`
*   **Announcements & Events:** `schoolannouncementintent`, `viewallannouncementsintent`, `schooltimingsintent`, `eventscheduleintent`, `addtocalendarintent`
*   **Contact & Support:** `contactinfointent`, `contactclassteacherintent`, `contactadminofficeintent`, `contacttransportdeptintent`, `contactaccountsdeptintent`, `escalatetohumanintent`
*   **Conversation Flow:** `Default Welcome Intent`, `backtomenuintent`, `closeconversationintent`, `youarewelcomeintent`, `goodbyeintent`, `downloaddocumentintent`, `Default Fallback Intent`

### How the contexts link everything together

> `attendencecheckintent` → **[attendence-context]** → `attendanceminimumintent`, `absenceconfirmintent`, `absenceeditintent`
> 
> `examscheduleintent2` → **[exam-context]** → `examresultintent`, `admitcardintent`, `setreminderintent`
> 
> `feedetailsintent` → **[fee-context]** → `feeduedateintent`, `feereceiptintent`, `paymentredirectintent`, `paymenthistoryintent`, `feeextensionintent`
> 
> `schoolannouncementintent` → **[announcement-context]** → `eventscheduleintent`, `viewallannouncementsintent`, `addtocalendarintent`

---

## 🎨 Rich responses & integration

Instead of boring text blocks, the bot uses Dialogflow's **Custom Payload** feature (JSON) to serve up interactive cards, icons, and quick-reply suggestion chips. 

This rich interface is delivered through **Dialogflow Messenger**, embedded directly into my custom frontend website. Getting the chips to function smoothly was the trickiest part—since a chip tap just re-sends its label as a message, I had to ensure literally every button leads to a functioning, trained intent so the user never hits a dead end or triggers the fallback intent accidentally. 

---

## 🚀 Deployment

1.  Agent built and trained natively in the [Dialogflow ES Console](https://dialogflow.cloud.google.com/).
2.  Deployed via **Integrations → Dialogflow Messenger**.
3.  Embedded live on the custom project website using the Dialogflow Messenger script tag.
4.  Hosted via Netlify Drop to bypass local browser security restrictions and enable live API communication.

---

## ⚠️ Being upfront about limitations

Since this is an academic project, it doesn't hook into a live school database. The attendance figures, fee amounts, and exam dates the bot returns are sample data. In a real-world deployment, this would be wired up via webhooks to the school's actual student information system. The download and payment buttons currently point to functional mock pages I designed for the website to demonstrate the intended user flow, and are clearly labeled as demos rather than live transactions.

---

## 👤 About me

*   **Name:** Aaryan Odedra
*   **Registration Number:** 2305026
*   **Course:** Artificial Intelligence: Real-World Applications and Implications (Summative Assessment)
*   **Grade:** IB Grade 11
*   **School:** Udgam School for Children
