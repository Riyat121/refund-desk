# Refund Policy
Check the rules in this order. The first match wins.
1. FLAG_FRAUD (never auto-approve) if ANY of these is true:
   - amount is over $500
   - 3 or more refunds in the last 90 days
   - shipping and billing address do not match
2. DENY if the order was delivered more than 30 days ago.
3. AUTO_APPROVE if the amount is under $50.
4. NEEDS_HUMAN if the amount is $50 to $500.
