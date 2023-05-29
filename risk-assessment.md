# Risk assessment

Purpose of 2FA is to increase the security of primary account. However new risks arise that user should be aware of.

### Multisig - 2 out of 3

Any two accounts can sign transaction (also rekey transaction).

#### Primary account + recovery account&#x20;

If primary account is compromised, for example the web wallet is hacked, the user should know that the recovery account should not be created with the same wallet or be kept at the same secret store which might be compromised.

If recovery account is compromised, for example wife finds it in the vault, she should not be able to have access to the primary account.

#### Primary account + 2FA account

If primary account is compromised, if hacker is able to penetrate also the 2FA server, multisig account is compromised.

If 2FA server is compromised, if hacker is able to access primary account private key, multisig account is compromised.

At the time of writing, all 2FA servers are run by Scholtz\&Co, and the implementation in wallet is also run by Scholtz\&Co. Wallet as well as 2FA server is open source with docker images automatically built with dates in tags, and we urge other people to run the service as well. Please register your service at [https://github.com/scholtz/AlgorandPublicData/tree/main/2fa](https://github.com/scholtz/AlgorandPublicData/tree/main/2fa)

#### Recovery account + 2FA account

If recovery account is compromised (person has access to the valut), it is quite likely that the person has access to the mobile device. We recommend using authentication application which requires biometric input in order to show pin codes, for example microsoft authenticators.

Recovery account security should be more secure than primary account, so the compromise of the 2FA account and the recovery account is very unlikely.

### HW device with 2FA authentication

It is the main goal of this service to increase security of hardware devices. If mnemonic of the recovery phrase for ledger is leaked, it is much better to secure it with biometric signature.

Please keep in mind that secuirty of recovery account must be ensured. If the recovery account mnemonic is in the same vault as the ledger recovery mnemonic, person who get access to that vault has ability to sign transactions.

Interesting option is to create hot account for easy signing, secured with 2FA server, and use the ledger as recovery device.

### Multiple person security

It is possible to use the primary account and 2fa account with combination with other persons accounts.&#x20;

For example combination of 3 persons - 3 primary accounts, 3 2FA accounts. Threshold of 3 might mean that compromise of the server (if single 2fa server is used) might sign msig tx. Threshold of 4 might mean that if 2FA service goes down users does not have ability to sign txs. It is recommend for corporate security to run their own 2FA servers.

Combination of 9 persons - 9 primary accounts, 9 2FA accounts. Threshold of 9 might mean that compromise of the server (if single 2fa server is used) might sign msig tx. Threshold of 14 might mean that if 2FA service goes down users does not have ability to sign txs.&#x20;

It is possible that some of the persons do not use 2FA accounts, and might increase the security of the multisig.

### 2FA Account seed

Server secret is used to create the 2FA accounts. Hash(PrimaryAccount+Secret+SecondaryAccount) = seed for algorand account.

{% embed url="https://github.com/scholtz/Algorand2FAMultisig/blob/1ae2ccedf94f7d3d2de591de54c547555d7ed357/Algorand2FAMultisig/Controllers/MultisigController.cs#L146" %}

Authenticator secret key is Hash(TwoFactorAccount (generated from seed) + Secret)

{% embed url="https://github.com/scholtz/Algorand2FAMultisig/blob/1ae2ccedf94f7d3d2de591de54c547555d7ed357/Algorand2FAMultisig/Controllers/MultisigController.cs#LL335C17-L335C100" %}
