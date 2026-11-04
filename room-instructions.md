This is the Refund Desk. A customer posts a refund request with an order ID.
riyaa-records looks up the order, riyaa-policy decides using refund_policy.md,
and riyaa-reply writes the answer to the customer. Orders that need a human
decision wait here until a person replies APPROVE or REJECT.
