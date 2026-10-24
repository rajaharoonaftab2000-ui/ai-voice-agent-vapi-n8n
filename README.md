# AI Voice Receptionist — Vapi, n8n & MCP

An AI voice receptionist that handles appointment conversations, checks availability, and records confirmed bookings using Vapi, MCP, n8n, Google Calendar, and Google Sheets.

## Overview

This project demonstrates how a voice assistant can manage routine appointment requests through natural conversation.

The receptionist understands the caller’s request, collects the required details, and uses connected tools to check appointment availability. Once an available slot is selected and the caller confirms, the system creates the calendar appointment and saves the booking details.

Vapi manages the voice conversation. MCP gives the assistant access to the connected tools, while three n8n workflows provide the appointment automation behind the assistant.

## What It Does

- Handles appointment-related voice conversations.
- Collects the caller’s name and contact information.
- Identifies the requested service or appointment purpose.
- Collects the preferred appointment date and time.
- Checks availability using connected calendar tools.
- Handles unavailable slots by asking for another suitable time.
- Creates confirmed appointments in Google Calendar.
- Saves booking information in Google Sheets.
- Reads back the appointment details to the caller.
- Supports configurable business information and booking instructions.

## Architecture

**Caller → Vapi Voice Assistant → MCP Server → Connected n8n Workflows → Google Calendar / Google Sheets → Spoken Confirmation**

The assistant manages the conversation, while the connected workflows perform the booking actions.

## Three Connected Workflows

The receptionist connects to three n8n workflows that work together to support the appointment process.

Together, these workflows provide the tools needed to:

- Check the requested appointment time.
- Create an appointment after caller confirmation.
- Save the booking information for administrative tracking.

MCP exposes the configured workflow tools to the Vapi assistant. The assistant selects the appropriate tool during the conversation and uses its result to decide what to say next.

The exact tool names and distribution of actions across the three workflows depend on the imported workflow configuration.

## How It Works

### 1. Receive the Call

The receptionist greets the caller and asks how it can help.

### 2. Understand the Request

The assistant identifies the requested service or appointment purpose and asks the questions required by the business.

### 3. Collect Booking Details

The assistant collects the caller’s name, contact information, preferred date, and appointment time.

If required details are missing or unclear, it asks follow-up questions before proceeding.

### 4. Check Availability

The assistant calls the availability tool through MCP. The connected n8n workflow checks the requested slot against the configured calendar and booking rules.

If the requested time is unavailable, the assistant asks the caller for another suitable time and checks again.

### 5. Confirm the Details

Before creating the appointment, the assistant reads back the relevant booking details and requests the caller’s confirmation.

### 6. Create the Appointment

After confirmation, the connected workflow creates the appointment in Google Calendar.

The assistant should announce a successful booking only after the booking tool reports success.

### 7. Save the Booking Record

The connected automation records the booking details in Google Sheets for administrative tracking.

### 8. Provide Spoken Confirmation

The receptionist summarizes the confirmed appointment and asks whether the caller needs further assistance.

## Technologies Used

| Technology | Purpose |
| --- | --- |
| Vapi | Voice conversations and assistant tool calls |
| MCP | Access to connected workflow tools |
| n8n | Three connected automation workflows |
| Google Calendar | Availability checks and appointment creation |
| Google Sheets | Booking records and caller information |

## Setup Requirements

- A running n8n instance.
- The three connected n8n workflows.
- A configured Vapi assistant.
- An accessible MCP server endpoint.
- Google Calendar and Google Sheets credentials.
- A business calendar for appointments.
- A spreadsheet prepared for booking records.
- Business information and appointment rules.
- Correct timezone settings across the connected services.
- A supported calling or testing setup in Vapi.

## Configuration

1. Import the three workflow JSON files into n8n.
2. Connect your own Google Calendar and Google Sheets credentials.
3. Select the calendar used for appointment checks and bookings.
4. Configure the spreadsheet and booking-record columns.
5. Set the business timezone, working hours, and appointment duration.
6. Make the required workflow tools available through MCP.
7. Connect the MCP endpoint to the Vapi assistant.
8. Configure the assistant’s greeting, business information, and conversation instructions.
9. Explain when each tool should be called and which information it requires.
10. Enable the required workflows and test the full booking process.

Tool inputs, calendar settings, and spreadsheet fields must match the workflow configuration.

## Example Conversation

**Caller:**  
“Hi, I would like to book an appointment.”

**Assistant:**  
“Certainly. What is the appointment for?”

**Caller:**  
“A routine consultation.”

The assistant collects the caller’s details and preferred appointment time, then checks availability through the connected tools.

If the slot is available, it reads back the details and asks for confirmation. After the booking tool succeeds, it confirms the appointment to the caller.

The exact wording depends on the configured assistant instructions.

## Suggested Test Cases

- Request an available appointment.
- Request an unavailable time.
- Provide incomplete caller information.
- Give a weekday without specifying a date.
- Change the preferred time before confirming.
- Verify that the correct business timezone is used.
- Verify that the appointment appears in Google Calendar.
- Verify that the booking details appear in Google Sheets.
- Check how the assistant responds when a connected tool fails.
- Confirm that a failed booking is not announced as successful.

## Business Value

This project demonstrates how an appointment-based business can reduce repetitive call handling and manual booking entry.

The connected workflow structure separates conversation handling from business actions, allowing the voice assistant and appointment tools to be configured for different business needs.

## Customization

The solution can be adapted for clinics, salons, consultants, and other appointment-based businesses.

Customization options include:

- Business name and greeting.
- Voice and supported language.
- Services and appointment durations.
- Staff or provider calendars.
- Working hours and availability rules.
- Required caller information.
- Spreadsheet columns.
- Additional workflow integrations.

Each implementation requires configuration and testing for the intended business.

## Operational Notes

- Availability depends on the connected calendar and configured booking rules.
- Relative dates must be interpreted using the business timezone.
- Booking confirmation should follow a successful tool response.
- n8n and the MCP endpoint must remain accessible during use.
- Hosting, calling, and AI services may have separate usage charges.
- Credentials, authentication secrets, and customer data must be removed from public exports.

## Project Status

Portfolio demonstration of an AI voice receptionist connected to three n8n workflows for appointment availability, booking, and record management.

## Author

**Haroon AI Automation**

Practical AI assistants and n8n workflows for business automation.

## Demo Video

[Watch the AI Voice Receptionist Demo](./Flower-Bouquet-Voice-Agent-Clean-Call-Captions.mp4)
