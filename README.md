# AI-Powered Invoice Processing & Accounts Payable Automation

## Overview

This project is an AI-driven Accounts Payable (AP) automation system that processes vendor invoices directly from email, extracts structured financial data using a Large Language Model (LLM), prevents duplicate entries using idempotent logic, and records transactions into a centralized ledger (Google Sheets acting as a lightweight database).

The system simulates a production-ready small business invoice intake pipeline and demonstrates real-world automation design principles including:

- Event-driven architecture
- AI-powered data extraction
- Multi-invoice parsing
- Deduplication (idempotent processing)
- Upsert database logic
- Automated email lifecycle management

---

## Business Problem

Small and mid-sized businesses often process vendor invoices manually:

- Employees read invoice emails
- Data is manually entered into spreadsheets
- Duplicate invoices are accidentally recorded
- No automated confirmation is sent
- No structured database system exists

This manual process is:
- Time consuming
- Error-prone
- Difficult to scale

---

## Solution Architecture

This system automates the entire workflow:

1. Gmail Trigger detects new invoice emails
2. Gemini LLM extracts structured JSON from unstructured email text
3. Custom JavaScript parses and splits multiple invoices per email
4. Unique dedupe keys prevent duplicate processing
5. Google Sheets performs append-or-update (upsert)
6. Confirmation email is automatically sent
7. Email is marked as read to prevent re-processing

---

## Workflow Diagram

<img src="Screenshots/n8n Workflow.png" width="800"/>

This is the complete event-driven pipeline built in n8n.

---

## AI Extraction Prompt

The system instructs the LLM to return structured JSON only.

<img src="Screenshots/AI Expression Input.png" width="800"/>

The prompt forces strict JSON formatting to ensure consistent parsing and downstream automation.

---

## Multi-Invoice JSON Parsing Logic

The LLM response is parsed and converted into multiple invoice objects using custom JavaScript.

### JSON Cleaning + Extraction

<img src="Screenshots/Javascript Code 1.png" width="800"/>

### Deduplication Key Logic

<img src="Screenshots/Javascript Code 2.png" width="800"/>

Each invoice generates a unique key:
emailId::invoice_number::vendor

This ensures:

- Safe re-execution
- No duplicate rows
- Idempotent workflow design

---

## Example Incoming Emails

<img src="Screenshots/Invoice received.png" width="800"/>

The system handles:

- Multiple invoices per email
- Different vendor formats
- Varying subject/body structures

---

## Google Sheets Ledger (Database)

<img src="Screenshots/Google Sheets Table.png" width="800"/>

Columns:

- date_received
- vendor
- invoice_number
- amount
- due_date
- category
- status
- dedupe_key

The sheet uses append-or-update logic based on the dedupe key, functioning as a lightweight AP database.

---

## Automated Confirmation Response

<img src="Screenshots/Automated Confirmation Response.png" width="800"/>

After successful processing:

- A confirmation email is sent
- The email is marked as read
- The workflow will not re-process the same message

---

## Key Technical Concepts Demonstrated

### 1. AI Integration
Uses Gemini LLM to extract structured financial data from unstructured email input.

### 2. Idempotent System Design
Workflow can be re-run safely without duplicating invoice entries.

### 3. Upsert Pattern
Google Sheets acts as a database using appendOrUpdate behavior.

### 4. Event-Driven Architecture
Email trigger initiates the entire automation pipeline.

### 5. Multi-Record Handling
One email can generate multiple database records.

---

## Sample Extracted Invoice JSON

```json
{
  "vendor": "Office Depot",
  "invoice_number": "1001",
  "amount": 245.90,
  "due_date": "March 10",
  "category": "Supplies"
}
