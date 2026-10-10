# Customer Support Ticket Priority Prediction and Automated Assignment System Using Agentforce

Salesforce DX source for a support-ticket prioritization and assignment application. The project uses a custom `Support_Ticket__c` object, Apex classification, least-loaded agent assignment, and a record-triggered Flow that creates one urgent follow-up Task for each high-priority ticket.

## Project Demo

[Watch the Project Demo](https://drive.google.com/drive/folders/1SyGdE2cqw6kdBoHqnuCBzOsz1SRcnYr9?usp=sharing)

## Project Documents

- [Project document](docs/customer_support_ticket_priority_prediction_and_automated_assignment_system_using_agentforce.pdf)
- [Project document (NM)](docs/customer_support_ticket_priority_prediction_and_automated_assignment_system_using_agentforce_nm.pdf)

## What is included

- Support Ticket object with customer/account, description, status, priority, priority reason, assigned agent, SLA, AI analysis, and resolution fields.
- Bulk-oriented Apex priority classifier and agent assignment service.
- Active record-triggered Flow for idempotent high-priority Task creation.
- All, High Priority, Open, My Assigned, and Recently Created list views.
- Support agent permission set; assign it only to approved support agents.
- Lightning app navigation and a base record page definition.
- Agentforce setup guide and UI configuration instructions.

## Setup

1. Install Salesforce CLI and authenticate to a **sandbox**: `sf org login web --alias support-dev --instance-url https://test.salesforce.com`.
2. Assign `Support_Ticket_Agent` to intended ticket agents before testing automated assignment.
3. Validate: `sf project deploy validate --source-dir force-app --target-org support-dev --test-level RunLocalTests`.
4. Deploy: `sf project deploy start --source-dir force-app --target-org support-dev`.
5. Assign the `Support_Ticket_Agent` permission set to agents; assign object access to other intended users through a separately reviewed permission set/profile.
6. Complete the Lightning App Builder steps in [UI/UX guide](docs/ui-ux.md), since dashboard reports and page activation depend on the org's data and app assignments.

## Screenshots

### Support Ticket List
Shows the Salesforce Support Tickets object with created support ticket records.

<img width="1051" height="522" alt="Screenshot 2026-10-10 225617" src="https://github.com/user-attachments/assets/9cf96a26-091a-4ff5-aeef-bf6dbcba4447" />



### Support Ticket Details
Shows the Support Ticket record including ticket number, priority, status, assigned agent, SLA status, description, AI analysis, and priority reason.

<img width="1063" height="527" alt="Screenshot 2026-10-10 225632" src="https://github.com/user-attachments/assets/4f2a71e1-ad92-43eb-8879-abd77c7c9acb" />


### Salesforce Field Sales App
Shows the Salesforce Field Sales App interface and CRM functionality.

<img width="1051" height="527" alt="Screenshot 2026-10-10 225640" src="https://github.com/user-attachments/assets/2ecfb2a7-3c6c-47d5-ad85-9a13f5c09cce" />


### Automated Follow-up Task
Shows the automated urgent support ticket follow-up task associated with Support Ticket `ST-00001`.

<img width="1046" height="528" alt="Screenshot 2026-10-10 225650" src="https://github.com/user-attachments/assets/c4ad0846-dcf0-492d-8d7d-53ac93ef605f" />


## Important implementation details

Priority classification runs before insert/update and overwrites the priority from the current description. Assignment runs on insert only when no agent is supplied; it selects the lowest open-ticket count among active users assigned the designated agent permission set. If there are no designated agents, the ticket remains unassigned. The Flow runs after save, creates a Task once, and sets an idempotency flag. Agentforce features require org licensing and admin configuration; follow [docs/agentforce.md](docs/agentforce.md).

See [deployment](docs/deployment.md), [architecture](docs/architecture.md), and [requirements](docs/requirements.md) for operational detail.
