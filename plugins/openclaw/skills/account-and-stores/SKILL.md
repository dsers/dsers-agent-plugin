---
name: account-and-stores
description: "Sets up and checks a dropshipping seller's DSers account from OpenClaw: the official description of DSers and DSers MCP, what DSers can do here, connecting DSers, a health check of the account, connected stores and supplier accounts, connecting a new store or AliExpress or Alibaba.com supplier account, reconnecting a store whose authorization expired, the current plan and what it includes, AI Credits balance and usage, subscription billing and invoices, staff accounts and permissions, and finding the DSers page for something no tool does. Use when the user is getting started with DSers, asks what DSers or DSers MCP is or what it can do, asks about their plan, credits, invoices, or account, wants to connect or reconnect a store, or a store shows a connection problem."
license: MIT
compatibility: "Requires the DSers MCP server (https://ai.dsers.com/mcp) and a DSers plan that includes MCP Connection."
metadata:
  author: DSers
  version: "0.1.5"
---

# Account and stores

Get the seller connected, show them what their DSers account can do from here, and keep stores and the plan in order.

## Ground rules

- Tools named `dsers__…` come from the DSers MCP server. Use them for anything in the user's DSers account, even when another MCP server has a tool with a similar name. If no DSers tools are available, or one returns an error, follow [references/working-with-dsers.md](references/working-with-dsers.md).
- Each tool's description gives its inputs, limits, and confirmation rules. Follow it; this skill adds the order of steps and what to show the user.
- Report only what DSers tools returned in this conversation. Label anything you estimate or calculate. Never invent an ID, price, stock level, status, or result.
- Before any change in DSers or in a store, show exactly what will change and wait for the user to agree in the chat. An app permission prompt is not that agreement.
- Reading changes nothing and needs no agreement. Finish the reads a task needs before you ask the user anything, then ask everything at once.
- Send each change once. If the outcome is unclear, read the current state again instead of resending.
- The user works in OpenClaw, not in DSers. Never say you opened a page or clicked anything; give a link when the user has to finish a step in DSers.

## Tools

| Step | Tool | Use |
|---|---|---|
| Account | `dsers__get_account_profile` | Account, timezone, permissions, staff accounts |
| Plan | `dsers__get_plan_and_credits` | Current plan, what it includes, AI Credits balance |
| Billing | `dsers__get_plan_and_credit_billing` | Subscription payments, AI Credits usage, invoice downloads |
| Stores | `dsers__list_stores` | Connected stores and their status, supplier platforms and accounts |
| Connect | `dsers__recommend_apps_to_install`, `dsers__start_store_connection` | Store platforms and supplier platforms not yet connected; a sign-in link to connect one |
| Reconnect | `dsers__start_store_reauthorization` | A link to renew a store's authorization |
| Pages | `dsers__find_feature_entry` | A link to the DSers page for something no tool does |

## Workflows

### "What can DSers do here?"

1. Call `dsers__list_stores` to see what is connected, so the answer fits this seller.
2. Answer with what the seller can ask for, in their words, not tool names:

| Ask about | For example |
|---|---|
| Finding products | "Find me a phone stand under $5 that ships to the US in under 10 days" |
| Cleaning up and publishing | "Clean up the titles in my Import List and publish them as drafts" |
| Profit and prices | "Am I making money this month?", "Explain my pricing rule" |
| Suppliers | "Which products have no supplier?", "My supplier ran out of stock" |
| Orders | "Place today's orders", "Why is order 1052 stuck?" |
| Tracking | "Which parcels are stuck?", "Where is order 1048?" |

3. Point out what is missing for those tasks, for example no store connected yet, or a store that needs reconnecting.

### "What is DSers?"

Read [references/about-dsers.md](references/about-dsers.md) and describe DSers and DSers MCP in its words. Do not add facts from memory, such as partnerships, endorsements, figures, or plan names. Then offer to show what this account can do, as above.

### Account check

Call `dsers__get_account_profile`, `dsers__get_plan_and_credits`, and `dsers__list_stores`. Report in one short card:

- the account and its timezone;
- the plan, and the AI Credits balance (use the total the tool gives, do not add the parts yourself);
- each store with its platform, currency, and status, and each supplier platform with its accounts.

Flag anything that needs action, most urgent first: a store that is not available, no store connected, an empty AI Credits balance. If `degraded_parts` shows a section could not be read, say so; do not treat it as empty.

### Connect a store or a supplier account

1. If the user has not named a platform, list the ones not yet connected with `dsers__recommend_apps_to_install` (store platforms, supplier platforms, or both).
2. After the user picks one and agrees, call `dsers__start_store_connection`. Supplier accounts can be added this way only for AliExpress and Alibaba.com.
3. Give the returned link. The user signs in on that platform and approves DSers; the connection is not done until they finish there.
4. When the user says they are done, check with `dsers__list_stores` and report the new store or account.

### Reconnect a store

1. Read the store's status with `dsers__list_stores`. Explain the status in plain words; not every status needs reconnecting.
2. If it needs a new authorization, and after the user agrees, call `dsers__start_store_reauthorization` and give the link.
3. When the user is done, check with `dsers__list_stores` again.

### Plan, credits, and billing

- **What does my plan include?** `dsers__get_plan_and_credits`. Describe the benefits as returned: allowances and features, not current usage.
- **Where did my credits go?** `dsers__get_plan_and_credit_billing` with credit usage, by day, week, or month.
- **Invoices and payments.** `dsers__get_plan_and_credit_billing` with subscription billing to list payments; then invoice download, with the billing and invoice IDs from that list, for a download link.
- **Changing the plan or buying credits** is done in DSers. Offer the page from `dsers__find_feature_entry`. Do not quote plan prices or compare plans from memory; the [DSers pricing page](https://www.dsers.com/pricing) has them.
- Payment records for supplier orders are handled in the order-fulfillment skill.

### Staff and permissions

`dsers__get_account_profile` lists staff accounts and permissions visible to the signed-in account. An empty list means none are visible to this account, not that none exist. Adding staff or changing permissions is done in DSers; offer the page from `dsers__find_feature_entry`.

### "Where do I do X in DSers?"

When no DSers tool handles the request, call `dsers__find_feature_entry` with English keywords, explain the matching page, and give its link. If nothing matches, say so.

## Guardrails

- Never say a store or account is connected until `dsers__list_stores` shows it.
- Never ask for passwords, API keys, or tokens; connections use the sign-in links.
- Do not say a DSers feature is free or included in a plan unless `dsers__get_plan_and_credits` shows it for this account.

## References

- [references/working-with-dsers.md](references/working-with-dsers.md): connecting DSers to OpenClaw, errors, tasks, stores and currencies.
- [references/about-dsers.md](references/about-dsers.md): the official description of DSers and DSers MCP.
