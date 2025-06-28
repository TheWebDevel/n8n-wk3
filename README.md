# 🚦 Dual-Path Support Bot 🛠️

### n8n + Gmail + Jira – auto-triage in a snap ☕

* 🤖 **AI routing** – Claude-3 Sonnet (AWS Bedrock) inspects every chat and tags it **CUSTOMER** or **ADMIN** with urgency in a single pass.
* 🔗 **Lean data pulls** – `Chat Trigger` captures the message; an `HTTP Request` fetches the admin directory (`/api/v1/users`).
* 🔀 **Smart lane split** – two Filters choose the recipients:

  * **Customer lane** → users where `role = admin` *and* the address **doesn’t** contain “admin”.
  * **Admin lane** → users where `role = admin` *and* the address **does** contain “admin”.
* ✉️ **Comms sequence**

  1. Gmail **Send** – dispatches a polished HTML alert.
  2. Jira **Create Issue** – spins up a ticket with an AI-crafted subject and concise summary.
  3. Gmail **Reply** – answers in the same thread with a handy **“Jira Ticket”** button.
* 🧠 **Text generation stack** – three LangChain **Chain LLM** nodes produce the subject line, plain-text synopsis (72-char wrap), and compact HTML body.
* 📝 **Glue logic** – two tiny `Code` nodes bundle email list, subject, body, and metadata for each branch.
* ⚙️ **Stateless & simple** – nothing persists outside the run; everything rides in live HTTP and in-flow JSON.
* 🔐 **One-time creds** – AWS Bedrock, Gmail OAuth2, Jira Cloud API—set once in n8n and you’re ready.

> **Flow:** chat in → AI decides → email fired → Jira raised → thread replied — that fast. 🎉

### n8n Code/ Workflow
Link: https://github.com/TheWebDevel/n8n-wk3/blob/main/n8n-gmail-automation.json


![image](https://github.com/user-attachments/assets/d1140a2e-fead-4d43-a0f5-d1c58e1c6e1d)

### ⚠️ PoC Dataset Caveat

The `/api/v1/users` sample is inconsistent, so I adopted a pragmatic rule set to keep the demo clear:

| What the dataset shows                                | How this PoC treats it                                                     | Examples from the list                                 |
| ----------------------------------------------------- | -------------------------------------------------------------------------- | ------------------------------------------------------ |
| **`role: admin`** + email *contains* `admin@`         | *On-call / ops crowd* → gets **ADMIN** alerts (outages, P1)                | `admin@mail.com`                                       |
| **`role: admin`** + email *does NOT* contain `admin@` | *General internal team* → receives **CUSTOMER** queries (billing, feature) | `Diego@mail.com`, `Kaci33@yahoo.com`, `beko@gmail.com` |

All other records (`role: customer`) represent actual customers and are **never e-mailed** by the workflow.

> This split is speculative—chosen only to satisfy the filtering requirement with the messy data provided.


### Customer Demo
https://github.com/user-attachments/assets/05da0916-e210-4faf-aae6-10dae0945f92

### Admin Demo
https://github.com/user-attachments/assets/31082cf8-189b-40b2-a89d-c32cf0a88b94




