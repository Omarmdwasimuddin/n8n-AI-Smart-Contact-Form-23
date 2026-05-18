# 🤖 n8n — AI Smart Contact Form Guide

> n8n, OpenAI ও Supabase ব্যবহার করে একটি AI-Powered Smart Contact Form তৈরির ধাপে ধাপে গাইড।

🔗 **পূর্বশর্ত:** [Supabase Integration](https://github.com/Omarmdwasimuddin/n8n-Database-22)

---

## 📊 Workflow Overview

```
index.html (Contact Form)
      │  POST Request
      ▼
Webhook Node (contact-form)
      │
      ▼
OpenAI: Message a Model (GPT-4o-mini)
[Message Analyze করো → JSON Output]
      │
      ▼
Supabase: Create a row
[Database এ Save করো]
      │
      ▼
IF Condition
[Priority = High?]
      │
      ├── ✅ True  ──► Gmail: Send Alert Email
      │
      └── ❌ False ──► (কিছু করো না)
```

---

## 🌐 Webhook সেটআপ

### ✅ ধাপ ১ — Webhook Node যোগ করো

- **"Add first step"** এ ক্লিক করো
- **"Webhook"** সার্চ করো এবং ক্লিক করো
- নিচের Value দাও:

| Field | Value |
|-------|-------|
| **HTTP Method** | `POST` |
| **Path** | `contact-form` |

- **Test URL** কপি করো

---

### ✅ ধাপ ২ — index.html এ URL Paste করো

- VS Code Editor এ `index.html` ওপেন করো
- কপি করা **Test URL** Paste করো
- **Execute Workflow** বাটনে ক্লিক করো
- `index.html` কে **Live Server** দিয়ে ওপেন করো
- Form এ Data দিয়ে **"Send Message"** বাটনে ক্লিক করো

> ✔️ Webhook এ Data আসলে n8n তে দেখা যাবে।

---

## 🤖 OpenAI সেটআপ

### ✅ ধাপ ৩ — OpenAI Node যোগ করো

- **Webhook** এর **`+`** বাটনে ক্লিক করো
- **"OpenAI"** সার্চ করো এবং ক্লিক করো
- **"Message a model"** সিলেক্ট করো
- **"Set up credential"** এ ক্লিক করো

---

### ✅ ধাপ ৪ — OpenAI API Key নাও

- 🔗 [platform.openai.com](https://platform.openai.com/home) ভিজিট করো (Login / Sign Up করো)
- **"Create API Key"** এ ক্লিক করো
- একটি **Name** দাও
- **"Create secret key"** বাটনে ক্লিক করো
- **API Key** কপি করো
- n8n এর Credential এ **Paste** করো এবং **Save** করো

---

### ✅ ধাপ ৫ — OpenAI Node Value সেট করো

| Field | Value |
|-------|-------|
| **Model** | `GPT-4o-mini` |
| **Prompt** | `prompt.txt` এর Prompt + `{{ $json.body.message }}` |
| **Output Format** | `JSON Object` |

**Prompt এ কীভাবে Message যোগ করবে:**

```
prompt.txt এর Prompt এর শেষে লিখো:

Message: {{ $json.body.message }}
```

- **"Add option"** এ ক্লিক করো
- **"Output Format"** সিলেক্ট করো
- Type এ **"JSON Object"** দাও
- **"Execute step"** এ ক্লিক করো

> 💡 **JSON Object Output কেন?** OpenAI কে বলা হচ্ছে Structured JSON এ উত্তর দিতে — যেমন Priority, Sentiment, Summary ইত্যাদি।

---

## 🗄️ Supabase সেটআপ

### ✅ ধাপ ৬ — Supabase Node যোগ করো

- **Message a model** এর **`+`** বাটনে ক্লিক করো
- **"Supabase"** সার্চ করো এবং ক্লিক করো
- **"Create a row"** সিলেক্ট করো

---

### ✅ ধাপ ৭ — Supabase SQL দিয়ে Table তৈরি করো

- Supabase Dashboard এ যাও
- **SQL Editor** Tab এ ক্লিক করো
- `query.sql` ফাইলের Query **Paste** করো
- **"Run"** বাটনে ক্লিক করো

> ✔️ Table তৈরি হয়ে যাবে।

---

### ✅ ধাপ ৮ — Supabase Row এ Data সেট করো

- **Create a row** Node এ ফিরে আসো
- পাশ থেকে Value **Drag & Drop** করে Field এ বসাও:

| Supabase Field | n8n থেকে আসা Value |
|----------------|---------------------|
| `name` | `{{ $json.body.name }}` |
| `email` | `{{ $json.body.email }}` |
| `message` | `{{ $json.body.message }}` |
| `priority` | OpenAI Output থেকে `priority` |
| `sentiment` | OpenAI Output থেকে `sentiment` |
| `summary` | OpenAI Output থেকে `summary` |

- **"Execute step"** এ ক্লিক করো

> ✔️ Supabase Dashboard এ গিয়ে Table চেক করো — নতুন Row দেখা যাবে।

---

## 🔀 IF Condition ও Gmail সেটআপ

### ✅ ধাপ ৯ — IF Node যোগ করো

- **Create a row** এর **`+`** বাটনে ক্লিক করো
- **"if"** সার্চ করো এবং ক্লিক করো
- Condition সেট করো:

```
priority  equals  high
```

*(OpenAI যদি Priority = "high" দেয়, তাহলে Alert Email পাঠাবে)*

---

### ✅ ধাপ ১০ — Gmail Alert যোগ করো

- **IF** এর **True** এর **`+`** বাটনে ক্লিক করো
- **"Gmail"** সার্চ করো → **"Send a message"** সিলেক্ট করো
- নিচের মতো Value সেট করো:

| Field | Value |
|-------|-------|
| **To** | Admin এর Email |
| **Subject** | `🚨 High Priority Contact — {{ $json.body.name }}` |
| **Message** | AI Summary ও Contact Details |

---

## 💡 এই Workflow এ কী কী হচ্ছে?

```
১. User → Contact Form এ Message লিখলো
          │
          ▼
২. Webhook → Form Data পেলো
          │
          ▼
৩. OpenAI → Message পড়ে বিশ্লেষণ করলো
   └── Priority: high / medium / low
   └── Sentiment: positive / negative / neutral
   └── Summary: সংক্ষিপ্ত বিবরণ
          │
          ▼
৪. Supabase → সব Data Database এ Save হলো
          │
          ▼
৫. IF → Priority "high" কিনা চেক করলো
          │
          ├── High → Admin কে তুরন্ত Email গেলো 🚨
          └── Low/Medium → শুধু Database এ থাকলো
```

---

## 📋 Quick Reference

| ধাপ | Node | কাজ |
|-----|------|-----|
| ১ | Webhook | POST URL তৈরি করো |
| ২ | — | index.html এ URL দাও ও Test করো |
| ৩–৪ | OpenAI | API Key নিয়ে Credential সেটআপ |
| ৫ | OpenAI | Model ও Prompt সেট করো |
| ৬ | Supabase | Create a row Node যোগ করো |
| ৭ | — | SQL দিয়ে Table তৈরি করো |
| ৮ | Supabase | Drag & Drop দিয়ে Data সেট করো |
| ৯ | IF | Priority = high হলে Alert |
| ১০ | Gmail | Admin কে Email পাঠাও |

### OpenAI Output এর Structure:

```json
{
  "priority": "high",
  "sentiment": "negative",
  "summary": "User is requesting urgent technical support"
}
```

---

*এই Workflow দিয়ে যেকোনো Contact Form এর Message AI দিয়ে বিশ্লেষণ করে Database এ সংরক্ষণ ও প্রয়োজনে Alert পাঠানো সম্ভব।*
