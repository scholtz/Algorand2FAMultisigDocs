# Diagrams

### Setup authenticator app

@startuml&#x20;

actor Alice #green

Alice->Wallet: Create Primary account Wallet->Alice: Primary account public key

Alice->Wallet: Create Recovery account Wallet->Alice: Recovery account public key

Alice->2FAServer: Get ARC-14 realm (/v1/Multisig/GetRealm) 2FAServer->Alice: Return realm (2FA#ARC14)

Alice->Wallet: Sign ARC-14 Tx with Primary account Wallet->Alice: SendSigned Auth Tx

Alice->2FAServer: Request 2FA secret, auth with primary account, secondary account as param (/v1/Multisig/SetupAuthenticator) 2FAServer->Alice: Returns QR code 2FAServer->Alice: Returns error if previously stored

Alice->2FAServer: Confirm 2FA secret, auth with primary account, secondary account as param, pin as param (/v1/Multisig/ConfirmSetupAuthenticator) 2FAServer->Alice: Ok if stored correctly

@enduml

<figure><img src="http://cdn-0.plantuml.com/plantuml/png/fL3DJW8n4BxtAIRX8Wc4O3Xn86n8z6JKNHFFjJj0GxTTfrDqtzvPVWmYgoOUEzz_vv4rSQgSqeO3GUQiGUYj2D4hjNiDPy_QEUGfB0Wr8poGhJGre8q9oRFQmyFPZZqzXw5EBmB01fikmnnDs6AtuodUPNbzanL8mfh2BJ9a-M8ude3ukmgkHjlnw2uvjj6kHWBdxRclFZKdIPp87sM2zyHeFEb_QrxObJ-6FBt3c-NrR_zIe2zR6PQG9LwBx5Bv75yJHWOvGKH3o0FGGjz7r5yZ1Yqb-FBa13f2hKcVhovaMTtcecThD0VgtX_XCGSJIlo1WKI1m2wb4cvjID4r4CKLoqkh5i4lBLN_NpM0slgkZOUKlqsztqZxGsKPIuI6NQFK77sPAlS1" alt=""><figcaption><p>Setup authenticator</p></figcaption></figure>



Person creates primary account and recovery account. It is recommended that two accounts are created using different technologies, and recovery account is cold storage account.

In first request person request the 2FA server for the realm. He signs the authentication transaction, and uses it further with communication with 2FA server. 2FA server now knows the user has private key to the primary account.

To create 2FA account, user calls SetupAuthenticator method with parameter of recovery account. Server responds with QR code and manual entry key.

For increased user friendliness user can call this method as many times he wants until he call ConfirmSetupAuthenticator. When server stores information that this primary account and recovery account is used, it does not show sensitive qr code any more.

Server uses hash of its own secret with primary account and secondary account as seed for 2FA account.&#x20;

### Sign transaction with authenticator app

@startuml

actor Alice #green

Alice->Wallet: Sign msig tx with primary account Wallet->Alice: Signed msig tx

Alice->2FAServer: Get ARC-14 realm (/v1/Multisig/GetRealm) 2FAServer->Alice: Return realm (2FA#ARC14)

Alice->Wallet: Sign ARC-14 Tx with Primary account Wallet->Alice: SendSigned Auth Tx

Alice->2FAServer: Request to sign msig tx. Tx is in parameter, primary account is identified by auth tx, secondary account is parameter, pin is provided in parameter (/v1/Multisig/SignWithTwoFactorPINMsigTx) 2FAServer->Alice: Signed msig tx

Alice->Blockchain: Send msig tx

@enduml



<figure><img src="http://cdn-0.plantuml.com/plantuml/png/XL5DJm8n4BtlhvY4YoGWmN3YG5XDC1umPDc4S-sECB4VE9q2_dkxouUu8NfityTxVPq6KIpHcYnJMZn3RaWZTDQCwBAiEVKdAsKCoXYMj7PW0wr13h0dsS2MoIh-0gMrZqwo8xC_QOH70LPdoSNlOPOlaNV8OtX6WRnuwWz7mAYCXRl1RZYOHoEKH8C45_LrDxjeBkO5IcHtbYMyauo6e-xjsgUGybHyyKznTDMfV1uJlRnPli3FY450F8IhQUxh50f03hQAbKL1xhMdQWWLEg5tIYbl2QY3vD23WDgxgiMzTYBNtB3V9OVgLqvhl_eDg_JUSkzdpGSlNbxd2IWFxKN_-Av7u_M7tYXons5-2DDqYjPy0m00" alt=""><figcaption><p>Sign 2FA transaction</p></figcaption></figure>

Alice can sign transaction only if she has access to the primary account private key and when she has access to the 2FA device to request pin.

###
