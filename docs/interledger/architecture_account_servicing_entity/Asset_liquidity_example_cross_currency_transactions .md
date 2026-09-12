**Step-by-Step Liquidity Flow: Cross-Currency Example**

![Asset Liquidity Example Cross Currency Transactions](./Asset_liquidity_example_cross_currency_transactions%20.jpg)

* **Initial State:** The Rafiki instance starts with two zero-scale asset liquidity accounts configured: **EUR Liquidity at 10** and **USD Liquidity at 50**.
* **Transaction #1 (Inflow & Settlement):**
* Rafiki receives €10 worth of Interledger packets from a peer, moving 10 EUR from the peer's liquidity account into your EUR asset liquidity account, raising your EUR balance to **20 EUR** ($10 + 10$).
* The exchange rate converts €10 to $12 USD; because the USD account has 50 USD available, Rafiki successfully deducts 12 USD, dropping your USD balance to **38 USD** ($50 - 12$) while funding the incoming payment liquidity account.


* **Transaction #2 (Attempt & Inflow):**
* A subsequent transfer arrives with packets worth €50 from the peer, tentatively moving 50 EUR into your EUR asset liquidity account to increase the balance to **70 EUR** ($20 + 50$).


* **Transaction #2 (Failure & Reversal):**
* Applying the updated FX rate yields a requirement of $55 USD against a current USD balance of only 38 USD; because debits cannot exceed available liquidity, the transaction fails.
* Due to the failure, the pending €50 transfer is rejected and reversed, restoring your EUR asset liquidity account back to **20 EUR** ($70 - 50$).