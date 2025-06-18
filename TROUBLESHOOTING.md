# Troubleshooting Example: Webhook Not Firing on Candidate Status Change

## Scenario Overview

A customer has integrated Ashby’s API to automate workflows when a candidate’s status changes. They report that their system is not receiving webhook events when a candidate’s status changes, even though the change is visible in the Ashby UI.

---

## Steps to Reproduce

1. Set up a webhook in the Ashby dashboard to POST to a test endpoint (e.g., https://webhook.site/).
2. Change a candidate’s status in the Ashby UI (e.g., move from “Interviewing” to “Offer”).
3. Monitor the test endpoint for incoming webhook payloads.

**Expected:**  
A POST request with the candidate status change payload is received at the endpoint.

**Actual:**  
No webhook event is received, despite the status change being reflected in the UI.

---

## Issue Explanation

This is a common integration issue where webhooks are not firing as expected. Possible causes include:

- Webhook misconfiguration (wrong event type, incorrect endpoint URL, disabled webhook).
- API-level issues (rate limiting, authentication errors, payload formatting).
- Internal Ashby processing delays or failures.
- Customer endpoint issues (firewall, downtime, incorrect handling).

---

## Troubleshooting Steps

1. **Verify Webhook Configuration**
   - Check the webhook settings in the Ashby dashboard:
     - Is the webhook enabled?
     - Is the correct event type (e.g., `candidate.status.changed`) selected?
     - Is the endpoint URL correct and reachable?
   - Review the [Ashby API documentation on webhooks](https://developers.ashbyhq.com/) for required configuration.

2. **Check Webhook Delivery Logs**
   - Use Ashby’s webhook delivery logs (if available) to see if events are being sent or if there are delivery errors (e.g., 4xx/5xx responses).
   - If logs show delivery attempts, inspect the response codes and payloads.

3. **Test the Endpoint Independently**
   - Use a tool like `curl` or Postman to POST a sample payload to the customer’s endpoint to confirm it is reachable and responds with 2xx.

4. **Simulate the Event**
   - Trigger the candidate status change again and monitor both Ashby’s logs and the endpoint.
   - If possible, use Ashby’s “Test Webhook” feature to send a sample event.

5. **Check for API Rate Limiting or Errors**
   - Review Ashby’s API documentation for rate limits or error handling.
   - Ensure the webhook is not being throttled or blocked due to excessive requests.

6. **Escalate to Engineering (if needed)**
   - If the webhook is configured correctly, the endpoint is reachable, and no events are logged as sent, escalate to Engineering.
   - Provide Engineering with:
     - Webhook configuration details
     - Timestamps of status changes
     - Delivery log excerpts
     - Customer account info

---

## Solution

**Root Cause:**  
The webhook was configured for the wrong event type (`candidate.created` instead of `candidate.status.changed`).

**Resolution Steps:**

1. Guided the customer to update the webhook configuration to listen for the correct event type.
2. Used the “Test Webhook” feature to confirm delivery to their endpoint.
3. Monitored subsequent candidate status changes to ensure events were received.
4. Documented the troubleshooting process and updated internal knowledge base for future reference.

---

## Key Takeaways & Best Practices

- **Systematic Troubleshooting:** Start with configuration, move to logs, test endpoints, and escalate with clear evidence.
- **Cross-Functional Collaboration:** Work closely with Engineering when the issue is not customer-facing or configuration-related.
- **Customer-Centric Communication:** Keep the customer informed at each step, explain findings, and provide actionable next steps.
- **Process Improvement:** After resolution, update documentation and consider product improvements (e.g., clearer webhook setup UI, more granular logs).

---

## References

- [Ashby API Documentation – Webhooks](https://developers.ashbyhq.com/)
