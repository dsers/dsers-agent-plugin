# Working with DSers

Read this when no DSers tools are available, when a DSers tool returns an error, or before the first change you make in the user's DSers account in a conversation.

## Contents

- Connecting DSers
- When a tool returns an error
- Making a change
- Tasks that finish later
- Stores, money, and time
- Things no tool can do
- Showing results

## Connecting DSers

If no DSers tools are available in this conversation, tell the user what they need and how to connect. Continue the original request once a DSers tool call succeeds.

What the user needs:

- A DSers account. [Sign up](https://accounts.dsers.com/accounts/register?utm_source=claude_plugin&utm_medium=ai_assistant&utm_campaign=dsers_plugin&utm_content=connect)
- A DSers plan that includes MCP Connection. Plans are listed on the [DSers pricing page](https://www.dsers.com/pricing).
- For tasks that touch a store, at least one store connected in DSers.

How to connect:

- **Claude on the web or desktop, including Cowork**: open Customize → Connectors, search for DSers, select Connect, and sign in to DSers in the window that opens. On Team and Enterprise plans, an owner adds DSers for the organization first.
- **Claude Code**: this plugin adds the DSers server. Run `/mcp`, select the DSers server, and sign in.

Never ask for a DSers password, API key, or token. If the user starts to paste one, tell them not to.

## When a tool returns an error

A DSers error has a `code`, a `message`, a `retryable` flag, and often a `hint`. Act on the code:

| Code | What to do |
|---|---|
| `UNAUTHORIZED`, `INSUFFICIENT_SCOPE` | The sign-in has expired or lacks a permission. Ask the user to reconnect DSers as described above, then continue. |
| `PLAN_RESTRICTION` | The plan, a quota, or the AI Credits balance does not cover this. Tell the user what the message says. If it says the plan does not include MCP Connection, send them to the pricing page. Do not work around it. |
| `FORBIDDEN` | This DSers account may not use that tool or record. Signing in again does not change it; tell the user. |
| `RATE_LIMITED` | For a daily limit, tell the user when it resets, from the hint, and do not call that tool again before then. For a short limit, wait the time given and retry once. |
| `VALIDATION_ERROR` | Fix the arguments using the message and hint. Never resend the same arguments. |
| `NOT_FOUND` | The record does not exist in this account, or the ID came from somewhere else. Look it up again with a list or search tool. |
| `UPSTREAM_UNKNOWN`, or a timeout after a change was sent | The change may have been saved. Read the current state before doing anything else, and do not resend the change. |
| `UPSTREAM_REJECTED` | DSers refused the change. Report the message; do not retry automatically. |
| Any other code | Retry once only if `retryable` is true; otherwise tell the user what failed, using the message. |

Some results come back with part of the data missing, for example a `degraded_parts` or `failed_sources` field, or one supplier platform failing while others answer. Use what came back, say what is missing, and do not repeat the whole call.

## Making a change

Every change in DSers or in a store follows the same steps:

1. **Read the current state** with the matching read tool, just before the change. Many write tools need a fresh `resource_version` or exact row IDs from that read.
2. **Show a preview**: the store, the exact records, each value before and after, and any side effect the tool's description lists, such as buyer-visible prices, a sync setting being switched off, AI Credits being used, or a message going to a buyer.
3. **Wait for the user to agree in the chat.** A permission prompt from the app is not agreement. If the user changes the plan, show the new preview.
4. **Send the change once.**
5. **Check the result** by reading the state again, or by following the task (below), and report what was saved, what was not, and why.

For a batch, show the whole plan as one table, get one agreement for it, then send as few calls as the tools allow. If the user agrees to only part of a plan, send only that part.

## Tasks that finish later

Some tools start a task and return `task_type` and `task_id` instead of a final result, for example publishing products, placing orders, bulk price updates, AI Mapping, and refreshing supplier orders.

- Tell the user the task has started, not that it is done.
- Poll `DSers:get_job_status` with the exact `task_type` and `task_id` until `terminal` is true. Each call can wait up to 20 seconds; stop after about two minutes and tell the user the task is still running and that you can check again.
- When it ends, report each item's outcome from `result`: succeeded, failed with its reason, or unknown.
- If a status is unknown, check the records themselves. Never start the same task again to find out.

Tools that only accept a request, such as refreshing a tracking number or sending a buyer notification, return a message and no task. Say the request was accepted, and how the user can check the outcome.

## Stores, money, and time

- Most tools need a `dsers_store_id`. Get it from `DSers:list_stores` and match the store the user names. If more than one store matches, ask. Never act on every store unless the user asked for all of them.
- A store whose status is not available may need reconnecting. Read its status before suggesting that.
- Show every amount with its currency. Order income is in the store's currency, supplier costs are usually in the DSers default currency, and supplier prices can be in yet another currency. Do not add or compare amounts in different currencies unless a tool returned a converted amount.
- Tools return times in UTC. When the user says "today" or "this week", use the account timezone from `DSers:get_account_profile`, and show times in that timezone.
- Copy IDs exactly from tool results. A store order number, a supplier order number, and a DSers order ID are different things.

## Things no tool can do

The user works in Claude, not in DSers. Never say you opened a page or clicked anything in DSers.

When a step can only be done in DSers, call `DSers:find_feature_entry` with English keywords for the page, such as `unpublish product`, and give the returned page as a link. If nothing matches, name the DSers menu to open. Use it only after checking that no DSers tool does the job.

## Showing results

- Start with the answer: what was found or changed, and what needs the user's decision.
- For many records, give counts by group first, then the details, and offer the rest when the list is long.
- Use Markdown tables where the app renders them; otherwise use numbered lists.
- Show product and order titles exactly as DSers returns them.
- Give product images, supplier pages, payment links, and DSers pages as links.
- Show a shipping address, phone number, or email only when the task needs it.
