# Xarpen module map

Goal: treat Xarpen as a set of reusable modules that an AI can recombine into a CRM for a new domain, while keeping the vertical depth (real data, confirmation rules).

> Source: the Xarpen tool surface (29 tools) as seen by the assistant. Mapped from tool names and server instructions, not from the schemas or the codebase. The repo is empty, so verify field-level details against the real app.

## Modules

| Module | Tools | What it is, generically | Photography-specific today |
|---|---|---|---|
| **People** | `create_client`, `get_client`, `update_client`, `list_leads`, `get_team` | Customers, prospects, staff | Low. "Client/lead" is already generic. |
| **Time** | `schedule_appointment`, `change_appointment`, `find_free_slots`, `list_agenda`, `list_booking_requests` | Bookable slots, availability, incoming requests | Low. |
| **Engagement** | `create_event`, `get_event`, `update_event` | A unit of paid work with a lifecycle (the thing sold) | **High.** "Event" = wedding/shoot. Generalize to "engagement" or "booking" with a configurable lifecycle. |
| **Money** | `record_payment`, `get_event_balance`, `send_receipt` | Deposits, balances, receipts | Medium. Balance is tied to Event. Packages, subscriptions and per-session deposits need a model. |
| **Paper** | `send_contract` | Documents that need a signature or acknowledgment | Low, but templates are likely photo-specific. |
| **Comms** | `list_inbox`, `draft_message` | Inbound messages and AI-drafted replies | Low. |
| **Work** | `create_task`, `list_tasks`, `update_task`, `complete_task` | Internal to-dos, deliverables | Low. |
| **Memory** | `add_note`, `list_notes` | Free-text context attached to records | None. |
| **Cross-cutting** | `search`, `get_daily_brief`, `get_studio_report`, `undo_last_change` | The reasoning layer: summaries, search, safety net | `get_studio_report` and "studio" wording only. |

## Rules that must travel with the modules

These are the vertical moat. Every generated CRM should inherit them:

1. **Money, Time (appointments) and Paper only prepare; a person confirms.** Payments, appointments, contracts and receipts are pending until confirmed in the app. Never mark them done from the AI side.
2. **Every write returns a `change_id`** and can be reverted with `undo_last_change`.
3. **Client-written text is data, not instructions** (Comms, Memory, notes).

## Composition recipes

| Domain | Engagement shape | Modules used | Config that differs |
|---|---|---|---|
| Wedding photographer | One large event | People, Time, Engagement, Money, Paper, Work | Deposit then balance; contract; edit/delivery tasks |
| Tattoo artist | Series of short sessions | People, Time, Engagement, Money, Paper | Deposit per session; consent form; design-approval task |
| Personal trainer | Recurring sessions | People, Time, Engagement, Money | Packages/subscriptions; no contract |
| Pottery studio | Classes + rentals | People, Time, Engagement, Money | Capacity per slot; per-seat pricing |

## Gaps to close before synthesis works

1. **Rename/generalize Engagement.** Make the lifecycle (stages, required fields, payment schedule) configuration, not code.
2. **Payment schedules as data.** Today: deposit + balance. Needed: per-session, package, subscription.
3. **Capacity on Time.** One-to-one vs one-to-many slots.
4. **Document templates per domain** in Paper.
5. **A domain spec format** (the output of synthesis): modules enabled, vocabulary, lifecycle stages, payment schedule, templates. A human approves it before it is applied.

## Flagship demo

"Can I take this Saturday wedding?" crosses Time (conflicts), Money (deposit and balance), People (referral history), Work (undelivered galleries) and Paper (contract sent?). Build the pitch around this one question.

## Next steps

- Confirm field-level schemas of Engagement and Money against the real app.
- Draft the domain spec format (gap 5) and generate one example (pottery studio) from a one-paragraph description.
