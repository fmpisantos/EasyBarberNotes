# Design Requirements for Backoffice Mobile App

This mobile app is a **backoffice tool** for an managing Barber/Hairdressed scheduling app. It should be user-friendly, professional. The focus is on **efficiency**, **clarity**, and **intuitive navigation**.

---

## General Design Guidelines
1. **Accessibility**: Include readable fonts, clear contrasts, and support for screen readers.
2. **Feedback**: Provide visual cues (e.g., loading spinners, success messages, and error notifications).

---

## Pages and Features

### **Login**
- Simple login form with fields for:
  - Phone number
  - Password
- "Forgot Password?" link.
- A modern, professional login screen with branding/logo.

---

### **Register**
- Form with the following fields:
  - Name
  - Phone number
  - Password
- Clear password requirements displayed.
- Button: "Register Account".
---

### **Establishments Management**
#### **Establishments List**
- A searchable and filterable list of establishments. Include:
  - Name
  - Address
  - Category
  - Status (Active/Inactive)

#### **Add/Edit Establishment**
1. **General Information**:
   - Upload images.
   - Input fields:
     - Name
     - Description (rich text editor)
     - Address (with integrated map picker)
2. **Services**:
   - List of existing services.
   - Button: "Add New Service".
   - Service form:
     - Name
     - Description
     - Duration (e.g., 30 mins, 1 hour)
     - Price
     - Category (dropdown)
3. **Employees**:
   - List of employees with:
     - Name
     - Role
     - Status
   - Buttons:
     - "Add New Employee" (Search by phone number).
     - "Remove Employee".
     - "View Schedule".
     - "View Appointments".

#### **Join Establishment**
- Search for establishments by:
  - Name
  - Address
  - Category
- Display results in a card/grid format with a "Join" button.

---

### **Schedule Management**
#### **General Schedule View**
- Weekly timetable display:
  - Columns: Days of the week.
  - Rows: Time slots.
  - Filters:
    - Establishment
#### **Calendar View**
- Day/Week view with events marked.
- Filters: Establishment
#### **Add/Edit Schedule**
- Form with fields:
  - Select Establishment
  - Select Date
  - Select Time
#### **Time Off/Exceptions**
- Form similar to Add/Edit Schedule but includes:
  - Option to leave Establishment field empty.

---

### **Appointments Management**
#### **Appointments List**
- Searchable, filterable list by:
  - Establishment
  - Date
- Display summary info:
  - Establishment
  - Client name
  - Service
  - Appointment time
#### **Appointment Details**
- Detailed view with:
  - Client information
  - Service details
  - Appointment time
  - Status (Active, Completed, Canceled)
- Actions:
  - Cancel appointment.
  - Message client.

#### **Add Appointment**
- Form to schedule an appointment:
  - Select Establishment
  - Select date and time
  - Select client or add client details (name and or phone number)
