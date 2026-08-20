# 🤖 Autonomous 3-Way Invoice Matching Agent (n8n + Local AI)

An Accounts Payable automation agent built with **n8n** that performs automated **3-Way Matching** across Vendor Invoices, Purchase Orders (PO), and Goods Receipts (GR).

Runs **100% locally** with zero cloud API token costs and complete financial data privacy.

---

## 📺 Live Video Demo
https://www.loom.com/share/8e9d59483a294ed9bf1f554bbd27edf9

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
