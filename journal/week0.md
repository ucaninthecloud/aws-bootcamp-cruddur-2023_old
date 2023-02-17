# Week 0 — Billing and Architecture
## Security ##
MFA was enabled for the root account
Enabled 1 CloudWatch Billing alarm if sepnd is over $10.00 USD.
Created 2 Budgets:
1. AWS BootCamp Credits with a Maximum of $3.00 and three thresholds of 50, 75 and 100% 
2. Zero-Spend Budget with a Maximum of $1.00
3. Created a new account with FullAdmin permissions and attached the Billing policy
4. Enabled MFA and created access key for non-root account
5. Logged in to the 
6. Completed [Conceptual Diagram](https://lucid.app/lucidchart/f1c2d8fa-3444-4e0e-9058-783a0db427ef/edit?viewport_loc=-612%2C-386%2C4039%2C1998%2C0_0&invitationId=inv_acf742dd-1df7-429f-82e6-0e8e2a3c57bc)in Lucid:

![Alt text](assets/2023-02-14-Conceptual_Design_Cruddr.png "Conceptual Design")

 
7. Completed Lucid [Logical Architectural](https://lucid.app/lucidchart/2d7bf0e1-b921-46c6-b905-c52e89db76be/edit?viewport_loc=-322%2C83%2C2994%2C1481%2C0_0&invitationId=inv_a38e7eae-3399-4c19-ac50-c61d7a022914) Diagram:

![Alt text](assets/2023-02-14-Logical_Design_Cruddr.png "Conceptual Design")

`One of the interesting things I needed to tweak a little bit was the Momento logo. Andrew showed how he was editing this via Visual Studio Code and removed a couple of tags. I ended up changing the fill from none to black on the first line as well as removing the fill="currentColor"> for both path tags. I added the momento_logo_origina.svg and momento_logo.svg within the assets directory`

