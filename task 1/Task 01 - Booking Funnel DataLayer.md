# Task 01 - Booking Funnel DataLayer Implementation

## Multi-Step Booking Funnel Tracking

The appointment booking journey consists of three steps:

1. Select Clinic Location and Specialty
2. Enter Patient Details
3. Confirm Booking

GTM cannot natively listen to multi-step form transitions. The front-end developer must implement explicit `window.dataLayer.push()` calls whenever a user successfully completes a step.

The Martech team defines the event schema, parameters, GTM triggers, and GA4 mappings, while the front-end team implements the dataLayer events.

---

# Step 1: Location + Specialty Selection

## Front-End Implementation

```javascript
window.dataLayer = window.dataLayer || [];

window.dataLayer.push({
    event: "booking_step_complete",
    step_number: 1,
    step_name: "location_specialty_selected",
    clinic_location: "Koramangala",
    specialty: "Knee Pain"
});
```

## GTM Trigger

```text
Trigger Type:

Custom Event

Event Name:

booking_step_complete

Condition:

step_number = 1
```

## GA4 Event Parameters

```text
step_number
clinic_location
specialty
```

---

# Step 2: Patient Details Submission

## Front-End Implementation

```javascript
window.dataLayer.push({
    event: "booking_step_complete",
    step_number: 2,
    step_name: "patient_details_entered",
    patient_name_present: true,
    phone_present: true,
    preferred_date: "2026-07-15"
});
```

## GTM Trigger

```text
Trigger Type:

Custom Event

Event Name:

booking_step_complete

Condition:

step_number = 2
```

## GA4 Event Parameters

```text
step_number
phone_present
preferred_date
```

---

# Step 3: Booking Confirmation

## Front-End Implementation

```javascript
window.dataLayer.push({
    event: "consultation_booked",
    step_number: 3,
    step_name: "booking_confirmed",
    clinic_location: "Koramangala",
    specialty: "Knee Pain",
    booking_id: "ORTHO-2026-1001"
});
```

## GTM Trigger

```text
Trigger Type:

Custom Event

Event Name:

consultation_booked
```

## GA4 Event Parameters

```text
booking_id
clinic_location
specialty
```

---

# GA4 Funnel Exploration Configuration

The funnel will be configured in GA4 Explore using the following sequence:

```text
Step 1:

Event:
booking_step_complete

Condition:
step_number = 1

↓

Step 2:

Event:
booking_step_complete

Condition:
step_number = 2

↓

Step 3:

Event:
consultation_booked
```

---

# Example Funnel Analysis

```text
Users Starting Booking Journey:

1000

↓

Completed Step 1:

750

↓

Completed Step 2:

500

↓

Completed Booking:

350
```

---

# Funnel Drop-Off Analysis

```text
Step 1 → Step 2:

25% Drop-Off

Step 2 → Booking Confirmation:

30% Drop-Off
```

This analysis enables the marketing and UX teams to identify friction points in the appointment journey.

Examples include:

- Mobile form usability issues
- Phone number validation errors
- Date picker complexity
- Slow page performance
- Excessive form fields

---

# Developer Implementation Notes

The front-end developer is responsible for implementing the `dataLayer.push()` statements after successful completion of each step.

The Martech implementation team defines:

- Event taxonomy
- Required parameters
- GTM triggers
- GA4 mappings
- Funnel exploration setup

This separation ensures accurate event tracking, maintainable code, and consistent analytics across all OrthoNow properties.