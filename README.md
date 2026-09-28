# AI-Restaurant-Ordering-WhatsApp-Assistance
Built an AI agent in n8n that runs a restaurant's WhatsApp ordering line, from the first message to a confirmed order, checking real stock and adding tax before anything gets sent to the customer.

Wrote the business rules in plain language instead of code. Tax rate, pickup time scaled to how big the order is, restaurant hours, when to send delivery to Uber Eats instead of trying to handle it, and a real phone number for anyone who wants to cancel or change an order.

Connected the agent to Google Sheets for inventory, FAQ answers, and order records. The FAQ answers now get pulled live off the sheet instead of duplicated in the prompt, so an edit to the sheet shows up in the very next reply instead of quietly going stale.

Once an order's confirmed, the workflow updates stock on its own, one item at a time, and flips anything that hits zero to out of stock. It also emails the restaurant a copy of the order, written by the model from the same details it just used to confirm everything, so staff have a record even if nobody's watching the WhatsApp line that minute.

The workflow generates order IDs, timestamps, and phone numbers on its own. The model handles judgment calls. Numbers that have to be exact every time go through code instead.

Added a LangChain agent on Google Gemini with rolling memory, so it doesn't ask a customer for their name twice in the middle of an order. Also added a filter that drops photos, voice notes, and reactions before they reach the model, since the system prompt has no instructions for any of that and would otherwise burn a paid API call for nothing.

Went through the actual spreadsheet the agent reads from and found the FAQ tab claiming home delivery the business rules didn't allow. After the sheet got updated, delivery and refunds lined up with the rules, but the hours still don't match, and one answer says no to online payments in the same breath it lists two card brands. Still catching these before calling the project done.
