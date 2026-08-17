# CampusConnect+ 🎓

**🌐 Live Project Link:** https://campusconnectplus.netlify.app/
**💻 GitHub Repository:** https://github.com/AaryanOdedra01/IDAI101-2305026--AaryanOdedra.git

---

Welcome to my submission for the **Creating Intelligent Chatbots for Practical Applications** assignment. I went with Scenario 2: ParentConnect AI, and built a virtual school communication assistant I've named **CampusConnect+**.

---

## 📌 What is this thing?

CampusConnect+ is meant to sit between parents and the school's administrative office, handling the questions that come up over and over: checking attendance, exam schedules, fee deadlines, circulars, the works.

The idea is simple — give parents instant answers without making them wait on hold or for an email reply back, while freeing up school staff to spend their time on the things that actually need a human. The conversational brain behind it all is built entirely in **Google Dialogflow ES**, running on its built-in Natural Language Understanding and Custom Payload responses. No external backend, no database — everything happens natively inside Dialogflow.

---

## 🖥️ The Frontend Website

I didn't want to leave this sitting in the Dialogflow test console, so I built a full, responsive frontend to actually host and present it properly.

* **Modern UI** — an animated gradient background paired with "glassmorphism" (frosted glass) cards, aiming for something that feels welcoming and professional rather than like a bare test widget.
* **Single-file architecture** — pure HTML, CSS, and JavaScript. No frameworks, no build step, nothing that needs a server to run.
* **A dedicated Mock Systems section** — since Dialogflow ES can't connect to a real school database or payment gateway, I built a section directly on the main website that simulates the documents and processes a parent would actually need: sample admit cards, fee receipts, term report cards, and a demo fee payment portal. Rather than having the chatbot try to link out to these directly from inside the chat window (which felt clunky and easy to break), the bot instead **tells the parent exactly where to find them** — pointing them to the Mock Systems section on the main page whenever they ask about downloading a document or paying a fee. It keeps the chat experience clean and keeps the actual "documents" living in one obvious, easy-to-find place on the site.
* **Print-ready documents** — each mock document has its own print stylesheet, so clicking "Print Document" hides all the site's background styling and colors, leaving just a clean white page — basically simulating what saving a real PDF would look like.

---

## 🧠 How the AI works (the short version)

When a parent types something — whether it's *"attendance for grade 8"* or something more casual and mixed like *"kab hai exams"* — Dialogflow maps it to the right **intent**, pulls out specific details (**entities**) like the grade level or subject, and if something's missing, politely asks for it through slot-filling before giving an answer.

The part I spent the most time on was making the conversation actually feel like a conversation, not a rigid form. If a parent asks about the exam schedule and then follows up with *"when will results come out?"*, the bot already remembers which grade they meant — it doesn't make them repeat themselves. That's handled through Dialogflow's **contexts**, which pass information from one intent to the next behind the scenes.

---

## 🛠️ Everything I built

I went well past the required 15 intents and 10 entities, mostly because once I started building out the "what happens when someone taps this button" flow properly, I kept finding places where a chip or a follow-up question needed somewhere real to land.

### Entities (11 custom + 4 built-in)

| Entity | What it captures |
|---|---|
| `@grade-level` | Grade/Class/Std from Nursery to 12, including the phrasing variants parents actually use |
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

Here's how the conversational paths break down:

* **Attendance:** `attendencecheckintent`, `attendancedetailedreportintent`, `attendanceminimumintent`, `absensereportintent`, `absenceconfirmintent`, `absenceeditintent`
* **Exams:** `examscheduleintent2`, `examresultintent`, `admitcardintent`, `setreminderintent`
* **Fees:** `feedetailsintent`, `feeduedateintent`, `feereceiptintent`, `paymentredirectintent`, `paymenthistoryintent`, `feeextensionintent`
* **Announcements & Events:** `schoolannouncementintent`, `viewallannouncementsintent`, `schooltimingsintent`, `eventscheduleintent`, `addtocalendarintent`
* **Contact & Support:** `contactinfointent`, `contactclassteacherintent`, `contactadminofficeintent`, `contacttransportdeptintent`, `contactaccountsdeptintent`, `escalatetohumanintent`
* **Conversation Flow:** `Default Welcome Intent`, `backtomenuintent`, `closeconversationintent`, `youarewelcomeintent`, `goodbyeintent`, `downloaddocumentintent`, `Default Fallback Intent`

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

Instead of plain text blocks, the bot uses Dialogflow's **Custom Payload** feature (JSON) to serve up interactive cards with titles, icons, and quick-reply suggestion chips. This is delivered through **Dialogflow Messenger**, embedded directly into my custom frontend website.

Getting the chips to actually function properly ended up being the trickiest part of the whole build. A chip tap just re-sends its label as a plain message — so if there's no intent specifically trained on that exact phrase, it silently falls into the fallback intent and the conversation just... stops making sense. I had to go through the entire flow and make sure every single button leads to somewhere real, and along the way ran into a few things worth mentioning:

* **Deprecated template-mode phrases** — Dialogflow used to let you type entities directly into a training phrase using `@` syntax. Turns out that mode is deprecated and gets silently ignored at training time, so a phrase could sit in the list looking completely normal while contributing nothing to matching. Took a while to catch since there's no obvious error — just an intent that mysteriously never triggers.
* **Context lifespans and missing output contexts** — a couple of my "linked" intents (like exam schedule → set reminder) weren't actually chaining properly because the earlier intent simply wasn't set to output the shared context in the first place. Small thing, easy to miss, took some digging through the diagnostic JSON to actually spot it.
* **Entity synonym gaps** — parents don't always answer in full phrases. When a slot-filling prompt asked "which fee?" and someone just replied "exam," it didn't match unless I explicitly added the short word as a synonym on the entity, not just the full "Exam Fee" label.

---

## 🚀 Deployment

1. Agent built and trained natively in the [Dialogflow ES Console](https://dialogflow.cloud.google.com/).
2. Deployed via **Integrations → Dialogflow Messenger**.
3. Embedded live on the custom project website using the Dialogflow Messenger script tag.
4. Hosted via Netlify Drop, which sidesteps local browser security restrictions and lets the widget actually talk to the live Dialogflow API.

---

## ⚠️ Being upfront about limitations

Since this is an academic project, it isn't hooked up to a real school database. The attendance figures, fee amounts, and exam dates the bot returns are sample data — a real deployment would wire this up via webhooks to the school's actual student information system.

The chatbot also doesn't link directly out to any download or payment page from inside the chat window — that felt like a fragile pattern to build around for a static, backend-less project. Instead, whenever a parent asks about downloading a document or paying a fee, the bot tells them the sample version is available under the **Mock Systems** section on the main website, where I've built functional (but clearly demo-labeled) versions of an admit card, fee receipt, term report card, and a sample fee payment portal.

---

## 👤 About me

* **Name:** Aaryan Odedra
* **Registration Number:** 2305026
* **Course:** Artificial Intelligence: Real-World Applications and Implications (Summative Assessment)
* **Grade:** IB Grade 11
* **School:** Udgam School for Children
