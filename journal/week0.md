# Week 0 — Billing and Architecture
## Security ##
MFA was enabled for the root account
Enabled 1 CloudWatch Billing alarm if sepnd is over $10.00 USD.
Created 2 Budgets:
1. AWS BootCamp Credits with a Maximum of $3.00 and three thresholds of 50, 75 and 100% 
2. Zero-Spend Budget with a Maximum of $1.00
3. Created a new account with FullAdmin permissions and attached the Billing policy
4. Enabled MFA and created access key for non-root account