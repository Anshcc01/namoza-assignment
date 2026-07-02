# Consultation Lead Capture Flow

I would implement a lightweight backend API between the landing page and third-party systems instead of using HubSpot embedded forms or no-code tools like Zapier.

```text
Landing Page (Vanilla JS)
        ↓
POST /api/consultation
(Node.js + Express Backend)
        ↓
HubSpot CRM API
(Create or Update Contact)
        ↓
Karix WhatsApp Business API
(Send Confirmation Message)
        ↓
Google Ads Conversion API
(Track consultation_form_submitted)
```

When a user submits the consultation form, the frontend sends the data to a backend API endpoint. The backend first checks HubSpot for an existing contact using the phone number. This is important because HubSpot's default deduplication works on email addresses, not phone numbers, and this form only captures name and phone. If a matching phone number exists, the contact is updated; otherwise, a new contact is created with the fields:

- Name
- Phone
- Clinic Preference
- Source = "Google Ads - Consultation Landing Page"
- Lead Status = "New Enquiry"

After the CRM operation succeeds, the backend triggers Karix's WhatsApp Business API to send a confirmation message to the patient. Finally, the consultation_form_submitted conversion is sent to Google Ads using the Google Ads Conversion API so campaigns can optimize toward actual lead submissions.

## Biggest Failure Point

The biggest failure point is phone-number deduplication in HubSpot. Since HubSpot does not treat phone numbers as unique identifiers by default, duplicate contacts could be created for the same patient. To prevent this, I would implement a custom lookup by normalized phone number before every create operation. If multiple records are found, the system would log the conflict and notify the operations team for manual review.

## WhatsApp SLA (Within 2 Minutes)

The two-minute SLA could fail because of HubSpot API latency, Karix service issues, network problems, or backend processing delays. To guarantee reliability, I would place WhatsApp requests in a lightweight message queue and implement automatic retries with exponential backoff. Monitoring would include application logs, API response-time dashboards, and alerts whenever the time between form submission and message delivery exceeds 120 seconds. This ensures that operational teams can react quickly if the SLA is breached.
