# Observações

O código parece estar correto, mas há um problema com a derivação da carteira HD. A derivação m/49'/1'/0'/0 é utilizada para derivar endereços BIP44, que são utilizados para carteiras de Bitcoin Legacy (P2PKH).

No entanto, a biblioteca bitcoinjs-lib utiliza a derivação m/44'/1'/0'/0 para derivar endereços P2PKH no testnet.

Além disso, é importante notar que a derivação m/49'/1'/0'/0 é utilizada para derivar endereços P2WPKH (SegWit) no testnet, e não P2PKH.

Para gerar endereços válidos no testnet, você pode atualizar o código da seguinte maneira:

javascript

```
// ...

// derivação de carteiras HD
const path = `m/44'/1'/0'/0`

// ...

let btcadress = bitcoin.payments.p2pkh({
  pubkey: node.publicKey,
  network: network,
}).address

console.log("Carteira gerada")
console.log("Endereço: ", btcadress)
console.log("Chave privada: ", node.toWIF())
console.log("Seed", mnemonic)
````

Isso deve gerar endereços válidos no testnet.

Se você quiser gerar endereços P2WPKH (SegWit) no testnet, você pode utilizar a derivação m/49'/1'/0'/0 e mudar a função bitcoin.payments.p2pkh para bitcoin.payments.p2wpkh:

javascript

```
// ...

// derivação de carteiras HD
const path = `m/49'/1'/0'/0`

// ...

let btcadress = bitcoin.payments.p2wpkh({
  pubkey: node.publicKey,
  network: network,
}).address

console.log("Carteira gerada")
console.log("Endereço: ", btcadress)
console.log("Chave privada: ", node.toWIF())
console.log("Seed", mnemonic)
```
Isso deve gerar endereços P2WPKH válidos no testnet.
