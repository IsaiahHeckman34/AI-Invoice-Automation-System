# AI-Powered Invoice Processing & Accounts Payable Automation

## Overview

This project is an AI-driven Accounts Payable (AP) automation system that processes vendor invoices directly from email, extracts structured financial data using a Large Language Model (LLM), prevents duplicate entries, and records transactions into a centralized ledger (Google Sheets acting as a lightweight database).

The system is designed to simulate a real-world small business finance workflow and demonstrates production-level automation patterns such as idempotent processing, deduplication logic, and event-driven architecture.

---

## Problem

Small and mid-sized businesses often process vendor invoices manually:

- Employees read invoice emails
- Data is manually entered into spreadsheets
- Duplicate invoices can be accidentally recorded
- No standardized tracking or automation exists
- Confirmation emails must be manually sent

This manual process is:
- Time consuming
- Error-prone
- Non-scalable

---

## Solution

This project automates the entire intake and processing pipeline:

1. Gmail Trigger detects new invoice emails
2. Gemini LLM extracts structured invoice data from unstructured email text
3. System parses multiple invoices per email
4. Deduplication logic prevents duplicate processing
5. Data is appended or updated in Google Sheets (AP ledger)
6. Processed emails are automatically marked as read and labeled

The result is a fully automated invoice intake system.

---

## Architecture

Workflow architecture:

Gmail → LLM (Gemini) → Code Parser → Deduplication → Google Sheets (Upsert) → Gmail (Mark Processed)

Key design principles:
- Event-driven trigger
- Single AI call per email
- Multi-invoice parsing
- Idempotent processing
- Database upsert pattern

---

## Tech Stack

- n8n (workflow automation engine)
- Google Gemini LLM (invoice data extraction)
- Gmail API (email trigger + lifecycle management)
- Google Sheets API (ledger storage & upsert)
- JavaScript (custom parsing & dedupe logic)

---

## Key Features

### 1. AI-Based Structured Data Extraction
Converts unstructured invoice emails into structured JSON:

```json
{
  "vendor": "Office Depot",
  "invoice_number": "5001",
  "amount": "120.45",
  "due_date": "March 5",
  "category": "Supplies"
}
