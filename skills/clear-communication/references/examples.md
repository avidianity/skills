# Before and After Examples

## Short message: review request

**Before**

> Hi, I wanted to check in about the supplier email that is in the shared draft.
> It would be great if you could take a look at it at some point today.
> The new delivery date is still tentative, so please review the wording about the date before I send the email tomorrow morning.

**After**

> Can you review the delivery-date wording in the shared draft today?
> The new date is still tentative, and I plan to send the supplier email tomorrow morning.

## Technical explanation: retrying invoice creation

**Before**

> When it comes to retries, a timeout can happen in between the server committing the database transaction and the client receiving the response.
> The API stores the invoice ID and the idempotency key in the same transaction, and keys are scoped to an account and kept for 24 hours.
> If the same account sends the same payload with the same key during that window, the API returns the original invoice ID.
> For the same account and key, a changed payload returns HTTP 409 while the key is retained.
> For a timed-out request, retrying during the retention window is safe with the same key and payload.
> Once the key expires, the same request can create a second invoice.

**After**

> After an invoice creation timeout, retry safely within the 24-hour retention window using the same account, idempotency key, and payload.
> Once the key expires, the retry can create a second invoice.
>
> A timeout can occur after the database commits and before the client receives the response.
> The API stores the invoice ID and key in the same transaction and scopes the key to the account.
> While the API retains the key, reuse it on the same account to get the original invoice ID for a matching payload or HTTP 409 for a changed one.

## Executive update: early customer churn

**Before**

> Over six months, we reviewed about 2,000 customer accounts and looked at when customers left and whether they had finished setup.
> Most customers who churned left within 60 days, and early departures correlated with incomplete setup.
> The pattern points more toward onboarding than pricing, although we cannot establish the cause from this analysis.
> One thing we could do is pilot a day-14 check-in for customers who have yet to complete onboarding.

**After**

> Most customers who churned left within 60 days.
> In a six-month review of about 2,000 accounts, early departures correlated with incomplete setup.
> The pattern makes onboarding the stronger lead compared with pricing, and the cause remains unverified.
>
> I recommend piloting a day-14 check-in for customers who have yet to complete onboarding.

## Informational update: no action required

**Before**

> After the overnight backup, we ran the scheduled restore check in staging.
> We verified that staging could read the restored database.
> The staging check passed, and it did not cover production.
> This is just an update, and nobody needs to do anything.

**After**

> The scheduled restore check passed in staging after the overnight backup.
> Staging could read the restored database.
> The check covered staging only.
> No action is needed.
