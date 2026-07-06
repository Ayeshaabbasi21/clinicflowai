# clinicflowai

## Overview

**clinicflowai** is an AI-powered patient lifecycle automation platform designed specifically for dental and medical clinics. The platform acts as a digital receptionist, orchestrating the entire patient journey—from initial inquiry and clinical qualification to automated appointment scheduling, post-treatment follow-ups, and reputation management. By integrating directly with clinical workflows via the WhatsApp Business API and n8n, clinicflowai reduces administrative burden, minimizes no-shows, and ensures consistent patient engagement.

## Features

*   **Intelligent Triage:** AI-driven qualification of patient inquiries to determine urgency and treatment requirements.
*   **Automated Scheduling:** Synchronized appointment booking directly through messaging channels.
*   **Lifecycle Orchestration:** Automated workflows for reminders, appointment confirmations, and rescheduling.
*   **Post-Treatment Follow-up:** Configurable messaging sequences for patient recovery instructions and wellness check-ins.
*   **Reputation Management:** Automated delivery of review requests following successful treatment completion.
*   **Referral Loop:** Proactive engagement to manage and track patient referrals.

## Tech Stack

*   **Backend:** FastAPI (Python 3.11+)
*   **Database:** PostgreSQL
*   **Automation Engine:** n8n (Workflow orchestration)
*   **Messaging:** WhatsApp Business API
*   **Containerization:** Docker & Docker Compose

## Architecture

The system follows a micro-service architecture:
1.  **FastAPI Backend:** Handles business logic, database transactions, and manages state for the patient lifecycle.
2.  **n8n Instance:** Acts as the integration glue, processing webhooks from the WhatsApp Business API and executing complex, multi-step automation logic.
3.  **PostgreSQL:** Stores patient profiles, clinical metadata, appointment logs, and interaction history.
4.  **Communication Bridge:** FastAPI interacts with the WhatsApp Business API to trigger messages, while n8n processes incoming signals to update the patient status in the database.

## Folder Structure

```text
clinicflowai/
├── app/
│   ├── api/            # API endpoints
│   ├── core/           # Configuration and security
│   ├── models/         # SQLAlchemy models
│   ├── services/       # Business logic (WhatsApp/n8n handlers)
│   └── main.py         # Application entry point
├── docker/             # Docker configurations
├── workflows/          # Exported n8n JSON workflow definitions
├── .env.example
├── docker-compose.yml
├── requirements.txt
└── README.md
```

## Installation

1.  **Clone the repository:**
    ```bash
    git clone https://github.com/yourusername/clinicflowai.git
    cd clinicflowai
    ```

2.  **Set up environment variables:**
    Create a `.env` file based on `.env.example`.

3.  **Start services:**
    ```bash
    docker-compose up --build
    ```

4.  **Configure n8n:**
    Access the n8n dashboard (default port 5678) and import the JSON files located in the `/workflows` directory to initialize the communication automation logic.

## Environment Variables

*   `DATABASE_URL`: PostgreSQL connection string (e.g., `postgresql://user:password@db:5432/clinicflow`)
*   `WHATSAPP_API_TOKEN`: Access token for the WhatsApp Business API.
*   `WHATSAPP_PHONE_NUMBER_ID`: Unique ID for the clinic's WhatsApp business account.
*   `N8N_WEBHOOK_URL`: The internal or public URL for n8n to receive events.
*   `APP_SECRET_KEY`: Secure key for FastAPI session management.

## API Overview

*   `POST /v1/inquiries`: Receives raw patient inquiries; triggers the AI qualification workflow.
*   `GET /v1/appointments`: Retrieves upcoming appointments or availability slots.
*   `POST /v1/appointments/schedule`: Creates a new booking and triggers a confirmation message via WhatsApp.
*   `PATCH /v1/patients/{patient_id}/status`: Updates the lifecycle state (e.g., "Inquiry", "Booked", "Post-Treatment").
*   `POST /v1/notifications/reminder`: Manually triggers a reminder sequence for a specific patient.

## Future Enhancements

*   **EHR Integration:** Direct synchronization with popular Electronic Health Record (EHR) systems like OpenDental or Dentrix.
*   **Multilingual Support:** AI-powered translation for patient communication in diverse demographic regions.
*   **Predictive Analytics:** Dashboard reporting on patient conversion rates and clinic bottleneck identification.
*   **Voice Integration:** Extension of the automation layer to handle inbound telephony via Twilio/VOIP providers.

## License

MIT License. See `LICENSE` for details.