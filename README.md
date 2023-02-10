# Verify Signature
sign by wallet, verify by smart contract. using web3js lib.

- Web3js - [source](https://github.com/mosi-sol/VerifySignature/blob/main/index.html) | [Live](https://mosi-sol.github.io/VerifySignature/) 
- Ethersjs - [source](https://github.com/mosi-sol/VerifySignature/blob/main/index-ethersjs.html) | [Live](https://mosi-sol.github.io/VerifySignature/index-ethersjs.html) 

---

## Example by Ethersjs & node js
```node
npm init -y
npm i alchemy-sdk @alch/alchemy-web3
npm i --save ethers
npm i --save dotenv
```

create index.js
```js
const main = async () => {
    require("dotenv").config();
    const { API_URL, PRIVATE_KEY } = process.env;
    const { ethers } = require("ethers");
    const { hashMessage } = require("@ethersproject/hash");
    const provider = new ethers.providers.AlchemyProvider("goerli", API_URL);
  
    const message = "Let's verify the signature of this message!";
    const walletInst = new ethers.Wallet(PRIVATE_KEY, provider);
    const signMessage = walletInst.signMessage(message);
  
    const messageSigner = signMessage.then((value) => {
      const verifySigner = ethers.utils.recoverAddress(hashMessage(message),value);
      return verifySigner;
    });
  
    try {
      console.log("Success! \nThe message: [\x1b[36m" +message+"\x1b[0m] \nwas signed with the signature: \n" +await signMessage);
      console.log("The signer was: \n" +await messageSigner);
    } catch (err) {
      console.log("Something went wrong while verifying your message signature: " + err);
    }
  };
  
  main();
```

create .env
```shell
API_URL = "https://eth-goerli.g.alchemy.com/v2/your_key"
PRIVATE_KEY = "0x00...00"
```

run node:
`node index.js`

---
enjoy it!
