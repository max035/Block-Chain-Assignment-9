# Block-Chain-Assignment-9
Financial Blockchain Capstone: African Remittances Payment Prototype
African Remittances Payment Prototype
A classroom prototype exploring how a blockchain-style ledger could support an inbound remittance flow. The project uses MiniRandCoin (mZAR) as a simulated fiat-referenced token and demonstrates the main steps of a payment: on-ramp, signed transfer, mining, verification, and off-ramp.
> **Disclaimer:** This is an educational simulation, not a production payment system. It does not connect to a bank, move real money, or prove that external reserve assets exist. Do not use real personal information, production secrets, or mainnet keys.
Project overview
The prototype models three roles:
Sender: receives simulated mZAR and initiates a transfer.
Recipient: receives mZAR and can redeem it through the simulated off-ramp.
Reserve wallet / vault: represents the reserve side of the toy ledger.
The reserve ledger follows a 1:1 accounting rule:
```text
vault_fiat == circulating
```
On-ramp: a simulated local-currency deposit increases the vault and issues the same amount of mZAR.
Transfer: mZAR moves between wallets. The reserve and total circulation do not change.
Off-ramp: redeemed mZAR is burned, the recorded vault balance is reduced by the same amount, and a payout receipt is returned.
The reserve invariant is an internal accounting check only. It does not establish that equivalent fiat funds are held by a real custodian.
Features demonstrated
Wallet creation for the sender, recipient, and reserve.
Simulated 1:1 on-ramp into mZAR.
Transfer acceptance checks, including signature verification and sender-balance checks.
Proof-of-work mining to include a pending transfer in a block.
Chain integrity verification and transaction lookup.
Simulated off-ramp and payout receipt.
A compact explorer view of the chain.
An optional Flask API wrapper for the remittance functions.
The notebook's signing example is explicitly a toy scheme. It is not production-grade public-key cryptography and should be replaced with a reviewed implementation such as ECDSA before any real-world use.
Demonstration flow
The notebook demonstrates the following example:
Create sender, recipient, and reserve wallets.
On-ramp 1,000 local-currency units to issue 1,000 mZAR.
Sign and submit a transfer of 750 mZAR from the sender to the recipient.
Mine a block containing the transfer and verify the chain.
Redeem 750 mZAR through the simulated off-ramp.
Check the resulting balances and reserve invariant.
In the example, after redemption, the sender has 250 mZAR, the recipient has 0 mZAR, and the recorded vault has 250 local-currency units. These are simulated ledger values, not real funds or a guarantee of redemption.
Repository contents
The implementation is currently organised as a Jupyter notebook. The core demonstration runs in-process; a persistent HTTP server is not required for that path.
```text
.
├── Lecture9.ipynb     # Notebook containing the prototype and demonstration
└── README.md
```
If the notebook is committed under a different filename, update the command below to match the filename in the repository.
Requirements
Python 3.10 or later recommended.
Jupyter Notebook or JupyterLab.
Flask is optional and is only needed for the API extension.
The core in-process demonstration uses Python code in the notebook. Install the optional API dependency with:
```bash
python -m pip install flask
```
To install JupyterLab if it is not already available:
```bash
python -m pip install jupyterlab
```
Running the prototype
Clone the repository:
```bash
   git clone <YOUR-REPOSITORY-URL>
   cd <YOUR-REPOSITORY-DIRECTORY>
   ```
Start JupyterLab:
```bash
   jupyter lab
   ```
Open `Lecture9.ipynb`.
Run the notebook cells in order, from the beginning through the demonstration and verification sections.
Review the printed wallet addresses, balances, mined block, verification results, receipt, and explorer summary.
The notebook's in-process flow does not require a long-running Flask server. The optional Flask section uses Flask's test client to exercise the API routes.
Optional remittance API
The notebook includes an optional Flask route wrapper. The documented route shapes are:
Method	Endpoint	Purpose
`GET`	`/`	Service information and endpoint list
`POST`	`/onramp`	Issue mZAR against a simulated deposit
`POST`	`/transfer`	Submit a signed wallet transfer
`POST`	`/mine`	Mine pending transactions
`POST`	`/offramp`	Redeem mZAR and return a simulated payout receipt
`GET`	`/balances/<address>`	Read a wallet balance
`GET`	`/chain`	Return the blockchain
`GET`	`/explorer`	Display a chain summary
`POST`	`/remit`	Optional orchestrated remittance flow
The API routes are an optional extension of the notebook, not a claim that a production-ready web service is configured. Request fields and response formats should be checked against the route implementation before using external clients.
For local development, bind any server to `127.0.0.1` or `localhost`. Do not expose this prototype publicly.
Core design checks
When reviewing the notebook output, confirm that:
The on-ramp issues the same amount of mZAR as the simulated deposit.
A transfer is only applied after it is included in a mined block.
Invalid signatures and insufficient balances are rejected by the relevant checks.
A transfer does not change the reserve balance or total circulation.
The off-ramp burns the redeemed amount and reduces the recorded vault balance accordingly.
The chain verification function passes for the untampered chain.
Limitations and risk considerations
This prototype simplifies several important parts of a real remittance system:
Reserves: the vault is an in-memory accounting representation. It is not connected to bank accounts, custody providers, or independent reserve attestations.
Depeg and redemption: a 1:1 ledger rule does not guarantee market-price stability, liquidity, or legally enforceable redemption.
Cryptography: the notebook's signing example is a toy scheme and is not suitable for securing real assets.
Identity and compliance: wallet addresses and valid signatures do not establish a user's identity or demonstrate AML/CFT compliance.
Persistence: the demonstration is notebook-based and in-process. Do not assume balances or chain state persist across restarts.
Network and finality: the prototype does not provide production-grade decentralised consensus, operational resilience, or legally defined settlement finality.
Security: the code has not been presented as independently audited or production-hardened.
A real deployment would require reviewed cryptography, secure key management, durable storage, reserve custody and assurance, robust transaction validation, monitoring, legal analysis, and compliance controls.
Educational purpose
The project is intended to make the remittance flow and its ledger invariants tangible, while illustrating the distinction between a recorded blockchain transfer and completed fiat redemption. It is a learning prototype and should not be treated as financial, legal, or security advice.
