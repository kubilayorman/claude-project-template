# Language Guide

Every document the skill writes follows these rules. The readers are non-technical stakeholders and junior developers. They should understand each sentence on the first read.

## Core Principles

1. **Clear over clever.** Use the plainest words that are still accurate.
2. **Short over long.** Say it once, in as few words as it takes.
3. **Concrete over vague.** Name the actual thing, number, or action.
4. **Business first, technical second.** Start with what happens and why it matters. Then give the technical detail.

## Sentences

- Keep sentences under 25 words. Split longer ones.
- Put one idea in each sentence.
- Use active voice: "The app sends a confirmation email", not "A confirmation email is sent".
- Use present tense: "This function creates a customer", not "This function will create a customer".
- Name who or what does the action: "The checkout page calls `create_order`", not "`create_order` is called".
- Start each section with one or two sentences a non-technical reader can follow. Put technical detail after that.

## Words

- Use everyday words. Write "use" not "utilize", "help" not "facilitate", "start" not "initiate", "show" not "display functionality for".
- Define each technical term the first time it appears, in a few words: "an API (a way for two programs to talk to each other)". After that, add the term to the Glossary in the Project Overview and use it without explaining.
- Call each thing by one name everywhere. If the code says `Client` but the business says "customer", pick one, explain the link once, then stick to it.
- Write names from the code in `backticks`: `customers` table, `create_order()` function, `src/services/billing.py` file.
- Use exact numbers when the code gives them: "retries 3 times", not "retries a few times".

## Banned: Marketing and Filler

Do not use these words or anything like them. They add no information.

| Do not write | Why |
|---|---|
| seamless, robust, powerful, cutting-edge, state-of-the-art, best-in-class, world-class | Marketing claims, not facts |
| leverage, empower, streamline, unlock, harness, supercharge | Vague verbs; say what actually happens |
| comprehensive, holistic, end-to-end solution, ecosystem | Buzzwords with no clear meaning |
| simply, just, easily, obviously, clearly, of course | They make readers feel slow when something is not simple for them |
| very, really, quite, basically, essentially, actually | Filler that weakens the sentence |
| "It is important to note that", "In order to", "It should be mentioned that" | Wordy; cut straight to the point |

## Honesty

- State facts as facts. Do not hedge with "might", "should", or "probably" when the code gives a clear answer.
- When the code does not give a clear answer, say so with `⚠️ Needs confirmation`. Do not guess, and do not hide the gap with vague wording.
- Do not praise or criticize the code. Describe it. Known problems belong in "Known Limitations", written as plain facts.

## Structure

- Use headings as plain labels: "Creating a Customer", not "The Magic of Customer Onboarding".
- Use lists for steps, options, or parallel items. Use short paragraphs for explanations.
- Use tables for repeated fields that readers compare (table fields, file index).
- Number steps that happen in order.
- Keep each description as short as it can be while still complete. One clear sentence beats three vague ones.

## Before and After

**Before:** "The `OrderService` leverages a robust, event-driven architecture to seamlessly orchestrate the end-to-end order lifecycle."
**After:** "`OrderService` creates orders, updates their status, and sends an event when an order is paid."

**Before:** "In order to facilitate customer onboarding, the system utilizes a comprehensive validation layer."
**After:** "Before creating a customer, the app checks that the email is valid and not already registered."

**Before:** "Data is persisted to the database and subsequently an email notification is dispatched."
**After:** "The app saves the order to the `orders` table, then emails the customer a receipt."

**Before:** "This function basically just handles the stuff related to invoices."
**After:** "This function creates a PDF invoice for a paid order and stores it in `invoices`."

## Final Language Check

- [ ] No sentence over 25 words.
- [ ] No banned words.
- [ ] Each section opens with a plain-language sentence.
- [ ] Each technical term is defined on first use.
- [ ] Each thing has one name throughout all three documents.
- [ ] No guessing; gaps are marked `⚠️ Needs confirmation`.
