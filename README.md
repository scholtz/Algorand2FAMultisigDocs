# Algorand 2FA Auth Documentation

This is documentation and security walkthrough for open source 2FA implementation for algorand multisig [https://github.com/scholtz/Algorand2FAMultisig](https://github.com/scholtz/Algorand2FAMultisig)

### Use cases

* Increase security for ledger device by adding fingerprint 2FA
* Increase security for hot account @ web wallet
* Corporate self custody security - 3 persons - 3 crypto devices + 3 2fa devices

### Solution

Algorand has native support for multisignature accounts. Multisignature account is set of private key protected accounts with defined signature threshold. Lets define 3 accounts. Primary account - the account for which we want to improve security. 2FA account - server api account which can sign transaction only if valid authentication pin is provided. Recovery account - account set for recovery purposes. Multisignature account is the account composited from these 3 accounts, where valid signature can be done using compounding signature of at least 2 accounts.

User can rekey multisignature account to standard account or any other mulitisignature account only if he is able to sign the transaction for the original multisignature account. In this case the original account address is not changed but at the blockchain the information about signator is changed and new set of signators can submit transaction on behalf of the original account.

There is variety of 2FA applications. Most common one is for example google authenticator, even though it lacks few security measures such as it is freely accessible to anyone who has access to the device. Other 2FA applications use more security, for example microsoft authenticator requires biometric signature (fingerprint) to be checked before it shows the 2fa pins.

Server API uses ARC-14 authentication. ARC-14 is authentication by signing self transaction with specific realm. Validity of such transaction is validated in scope of selected network and expiration is checked against the current round on the network vs signed validity round number.





&#x20;

