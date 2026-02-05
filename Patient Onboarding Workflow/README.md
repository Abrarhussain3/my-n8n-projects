# Patient Onboarding Workflow (n8n)

This repository contains an **n8n Patient Onboarding Automation** that listens to Google Form submissions, stores patient data, sends condition-based welcome emails, updates Google Sheets, and notifies assigned doctors.


# Overview

**Trigger:** Google Form → Google Sheets  
**Automation Tool:** n8n  
**Use Case:** Healthcare patient onboarding (PCOS, Diabetes, Hypertension, Overweight)


## Workflow Architecture (Step-by-Step)

### Step 1: Google Sheets Trigger
- Listens for **new row added** in Google Form responses sheet.
- Acts as the entry point of the workflow.

### Step 2: Check if Welcome Email Already Sent
- Uses `welcomeSent` column.
- If empty → continue.
- Prevents duplicate onboarding emails.


### Step 3: Edit / Normalize Fields
- Generates a unique `patientId` (e.g. `PT-170000000`).
- Maps form fields:
  - Name
  - Phone (WhatsApp)
  - Email
  - Health Condition
  - Assigned Doctor
  - Weight
  - Blood Pressure
- Initializes tracking fields:
  - `riskScore = 0`
  - `lastCheckIn = null`
  - `consecutiveMissed = 0`
  - `createdAt = ISO timestamp`


### Step 4: Store Patient in Central Database
- Appends patient record into **Patients_DB Google Sheet**.
- Acts as the master patient table.


### Step 5: Condition-Based Routing (Switch Node)
Based on **Health Condition**:
- PCOS
- Diabetes
- Hypertension
- Over Weight

Each condition sends a **customized welcome email**.


### Step 6: Send Welcome Email to Patient
- Uses Gmail node.
- Includes:
  - Patient name
  - Condition
  - Patient ID
  - Assigned doctor (where applicable)


### Step 7: Mark Welcome as Sent
- Updates original Google Form response sheet.
- Sets `welcomeSent = yes`
- Uses **Timestamp** as the matching key.


### Step 8: Fetch Assigned Doctor Details
- Looks up doctor from **Doctors Contact Information** sheet.
- Matches using Doctor Name.


### Step 9: Notify Doctor via Email
- Sends doctor an email with:
  - Patient name
  - Patient condition
- Confirms new patient assignment.


## Google Sheets Used

1. **Patient Onboarding Data**
   - Source form responses
   - Tracks `welcomeSent`

2. **Patients_DB**
   - Central patient database

3. **Doctors Contact Information**
   - Doctor name & email mapping


## Setup Instructions

1. Install and run **n8n**
2. Import workflow JSON:
   - `Patient Onboarding Workflow.json`
3. Connect credentials:
   - Google Sheets OAuth
   - Gmail OAuth
4. Ensure Google Sheets columns match workflow fields
5. Activate the workflow 


## Required Credentials

- Google Sheets OAuth2
- Gmail OAuth2


## Key Features

- Duplicate email prevention
- Condition-based logic
- Doctor notification
- Scalable patient tracking
- Fully no-code automation


## File/Links Included

- Patient Onboarding Workflow.json` – n8n workflow export
- 
  https://github.com/Abrarhussain3/my-n8n-projects/blob/main/Patient%20Onboarding%20Workflow/Patient%20Onboarding%20Workflow.json

  
- Patient Health Onboarding Form Link
  
  https://docs.google.com/forms/d/e/1FAIpQLSeP195p13NNBEhW3M9uQSjyYWlhqpML2FilKgrkz34jejkj4A/viewform?usp=header

  
- PATIENT ONBOARDING Data Sheet Link
  
  https://docs.google.com/spreadsheets/d/1I5oLFYVAzG1F8PDMwhF1UtNihlhNxEShWfV7JWw2Zpo/edit?usp=sharing


- Patients_DB SheetLink

  https://docs.google.com/spreadsheets/d/1O5Qjamo_9dEr4q6OPAQozkj2ddZl39vn29nyEpSGy8g/edit?usp=sharing


- Doctors  Contact information sheet Link
  
  https://docs.google.com/spreadsheets/d/1bJASWjmaGPKTbjCUo9kCQllBisT0aopvv_yJ8S72hpI/edit?usp=sharing






