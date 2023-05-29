# 2FA Server setup

Server can be build either from the source code, or latest docker imaga can be used.

Source code of the server: [https://github.com/scholtz/Algorand2FAMultisig](https://github.com/scholtz/Algorand2FAMultisig)

Docker tags: [https://hub.docker.com/r/scholtz2/algorand-2fa-multisig/tags](https://hub.docker.com/r/scholtz2/algorand-2fa-multisig/tags)

### Requirements

Storage is required in order to disable authentication app qr code generation after user confirms the pin. Storage capacity is approx 200 bytes per confirmed address in usage.

Standard RAM requirements per node is 50-100 MB, with safe max 1 GB.&#x20;

Server is not CPU sensitive, standard usage is below 0,1 vCPU, with safe max 1 vCPU.

### Recommendations

It is possible to use the server secret Algo:Mnemonic to be any text, but for future development it is recommended to generate one clean standard algorand account for it.

Secret can be set either in `appsettings.json` file or `Algo__Mnemonic` env variable.

`Secret is used to create the 2FA accounts. Hash(PrimaryAccount+Secret+SecondaryAccount) = seed for algorand account.`

Please configure your own `AlgorandAuthentication:Realm` config settings.

### Sample deployment

AWallet deployment in Germany:

{% embed url="https://github.com/scholtz/AlgorandNodes/tree/main/kubernetes/2fa/mainnet-2fa/de-awallet-2fa" %}

Aramid.Finance deployment:&#x20;

{% embed url="https://github.com/scholtz/Algorand2FAMultisig/blob/master/k8s/deployment-testnet.yaml" %}

### Algorand Public Data

After you configure your server, feel free to create PR to the Algorand Public Data github repository. From this repository wallets may directly obtain the list of 2FA servers which are available to public.

{% embed url="https://github.com/scholtz/AlgorandPublicData/tree/main/2fa" %}



