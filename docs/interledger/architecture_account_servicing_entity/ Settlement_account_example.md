**Settlement & Liquidity Flow: Credit Line and Payment Sequence**

![Settlement & Liquidity Flow: Credit Line and Payment Sequence](./%20Settlement_account_example.jpg)

* **Credit Extension & Deposit:** You allocate a **$10,000 USD line of credit** to your peer, which sets the peer liquidity account to **$10,000** and updates your internal USD settlement account balance to **-$10,000** to record the committed funds.
* **Payment Inflow:** An incoming payment of **$100 USD** is routed from the peer, consuming $100 of their extended line of credit.
* **Liquidity Withdrawal & Internal Crediting:** Because Rafiki does not directly hold customer balances, you withdraw the **$100** liquidity from Rafiki and credit the corresponding funds to the recipient’s balance on your external bank ledger.
* **Updated Ledger Balances:** Following the completion and withdrawal, the peer liquidity account balance drops to **$9,900**, while the USD settlement account adjusts toward zero to **-$9,900**, tracking the net funds now deployed.