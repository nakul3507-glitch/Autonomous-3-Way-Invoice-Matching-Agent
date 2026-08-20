# 🤖 Autonomous 3-Way Invoice Matching Agent (n8n + Local AI)

An enterprise-grade Accounts Payable automation agent built with **n8n** that performs automated **3-Way Matching** across Vendor Invoices, Purchase Orders (PO), and Goods Receipts (GR).

Runs **100% locally** with zero cloud API token costs and complete financial data privacy.

---

## 📺 Live Video Demo
https://www.loom.com/share/8e9d59483a294ed9bf1f554bbd27edf9

<img width="426" height="240" alt="invoice" src="https://github.com/user-attachments/assets/75c12bfc-7a24-4d55-b88b-76f515efb942" />




---

## 💡 The Problem & Solution
* **The Problem**: Manual 3-way invoice matching is tedious, error-prone, and expensive. Cloud AI solutions (like OpenAI APIs) expose confidential company rates and add ongoing token fees.
* **The Solution**: An autonomous local pipeline that parses incoming invoice PDFs, cross-checks ERP records in Google Sheets, auto-approves valid invoices, and alerts Slack for human review when discrepancies are found.

---

## 🏗️ Architecture & Pipeline

```text
[ Gmail (Invoice PDF) ]
         │
         ▼
[ Poppler CLI (pdftoppm) ] ➔ (Renders PDF to high-res PNG locally)
         │
         ▼
[ PaddleOCR Engine ]       ➔ (Local high-accuracy text extraction)
         │
         ▼
[ Local Qwen 3.5 4B (LM Studio) ] ➔ (Parses unstructured text into clean JSON)
         │
         ▼
[ Google Sheets ERP Lookup ] ➔ (Fetches matching PO and Goods Receipt records)
         │
         ▼
[ Smart "If" Node (3-Way Check) ]
         │
         ├── [ TRUE (Match Passed) ] ──➔ [ Log to Master Invoice Sheet ] ──➔ [ Auto Cleanup ]
         │
         └── [ FALSE (Mismatch) ]    ──➔ [ Dynamic Slack Alert ] ──➔ [ Log to Review Sheet ] ──➔ [ Auto Cleanup ]
```

---

## ✨ Key Features
* **100% Local AI Privacy**: Uses **PaddleOCR** and **Qwen 3.5 4B** via LM Studio. Zero invoice data is sent to third-party cloud AI APIs.
* **High-Speed Rendering**: Local **Poppler CLI (`pdftoppm`)** replaces slow cloud conversion nodes.
* **Dynamic Exception Handling**: Discrepancies (PO amount mismatch, quantity mismatch) trigger real-time **Slack alerts** diagnosing the exact error numbers.
* **Zero Disk Bloat**: Automated cleanup nodes purge temporary PDFs and PNGs from `C:\tmp` after every execution.

---

## 🛠️ Tech Stack
* **Automation Platform**: n8n (Self-Hosted)
* **Local Vision / OCR**: PaddleOCR
* **Local LLM**: Qwen 3.5 4B (via LM Studio / OpenAI-compatible API)
* **Document Processing**: Poppler CLI (`pdftoppm`)
* **Integrations**: Google Sheets API, Slack API, Gmail API

---

## 🚀 Quick Setup Guide

### 1. Prerequisites
* Install [Poppler for Windows](https://github.com/oschwartz10612/poppler-windows/releases) and add `bin` to PATH.
* Install [LM Studio](https://lmstudio.ai/), load `Qwen 3.5 4B`, and start local server on port `1234`.
* Create folder `C:\tmp` on your local drive.

### 2. Import Workflow into n8n
1. Start n8n with file access enabled:
   ```bash
   npx n8n
   ```
2. In n8n, click **Import from File** and select `workflow.json`.
3. Connect your Google Sheets and Slack credentials.
4. Execute and test!
