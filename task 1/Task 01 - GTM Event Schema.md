# Task 01 - GTM Event Schema

## GTM Event Tracking Plan for OrthoNow

| Event Name | Trigger Type | Key Parameters | GA4 Report / Audience |
|------------|--------------|----------------|----------------------|
| booking_step_complete | Custom Event (dataLayer) | step_number, clinic_location, specialty | Funnel Exploration |
| consultation_booked | Custom Event (dataLayer) | booking_id, clinic_location, specialty | Conversion Report |
| call_now_clicked | Click Trigger (tel:) | page_type, clinic_location, button_text | Engagement Report |
| whatsapp_clicked | Click Trigger (wa.me link) | page_type, clinic_location, source_page | High-Intent Audience |
| patient_guide_form_submitted | Form Submission Trigger | patient_name_present, phone_present, guide_name | Lead Generation Report |
| patient_guide_downloaded | Link Click Trigger (.pdf) | guide_name, file_url, clinic_location | Content Performance Report |
| clinic_page_view | Page View Trigger | clinic_name, city, page_url | Clinic Performance Dashboard |
| blog_scroll_25 | Scroll Depth Trigger (25%) | article_title, category, author | Content Engagement Report |
| blog_scroll_50 | Scroll Depth Trigger (50%) | article_title, category, author | Engaged Readers Audience |
| blog_scroll_75 | Scroll Depth Trigger (75%) | article_title, category, author | High-Intent Readers Audience |
| blog_scroll_100 | Scroll Depth Trigger (100%) | article_title, category, author | Fully Engaged Users Audience |
| consultation_form_submitted | Custom Event (dataLayer) | clinic_preference, source, phone_present | Google Ads Conversion Tracking |

---

## Booking Funnel Tracking Approach

The 3-step appointment booking form will be tracked using **custom dataLayer pushes implemented by the front-end developer**. GTM cannot automatically detect multi-step form state transitions, so explicit event pushes are required after successful completion of each step.

GA4 Funnel Exploration:

Step 1:

booking_step_complete

Condition:

step_number = 1

↓

Step 2:

booking_step_complete

Condition:

step_number = 2

↓

Step 3:

consultation_booked

This setup allows marketing teams to measure drop-off between steps and identify UX bottlenecks in the booking journey.

---

## Google Ads Conversion Import

### Selected Conversion

```text
consultation_booked
```

### Justification

I would import **consultation_booked** as the primary Google Ads conversion because it represents the highest-intent user action and is directly tied to business outcomes.

While events such as WhatsApp clicks or phone calls indicate engagement, a completed consultation booking provides the strongest optimization signal for automated bidding strategies such as:

- Max Conversions
- Target CPA
- Target ROAS

Using the final booking confirmation event helps Google Ads optimize toward actual patient acquisition rather than intermediate interactions.