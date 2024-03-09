# phantom-py
python for blockchain wallet phantom, suport use mnemonic seeds generate infinite solana, ethereum addresses and private keys

## pip
```
pip install bip_utils==2.7.0
```
## code
```
from bip_utils import Bip39SeedGenerator, Bip44Coins, Bip44, base58, Bip44Changes

class BlockChainAccount():

    def __init__(self, mnemonic, coin_type=Bip44Coins.ETHEREUM, password='') -> None:

        self.mnemonic = mnemonic.strip()
        self.coin_type = coin_type
        self.password = password # if have password

    def get_address_pk(self,accountNo):

        seed_bytes = Bip39SeedGenerator(self.mnemonic).Generate(self.password)
        if self.coin_type != Bip44Coins.SOLANA:
            # bip44_mst_ctx = Bip44.FromSeed(seed_bytes, self.coin_type).DeriveDefaultPath()
            bip44_mst_ctx = Bip44.FromSeed(seed_bytes, self.coin_type)
            bip44_mst_ctx.Purpose().Coin().Account(accountNo)
            return bip44_mst_ctx.PublicKey().ToAddress(), bip44_mst_ctx.PrivateKey().Raw().ToHex()
        else:
            bip44_mst_ctx = Bip44.FromSeed(seed_bytes, self.coin_type)
           
            bip44_acc_ctx = bip44_mst_ctx.Purpose().Coin().Account(accountNo)
            bip44_chg_ctx = bip44_acc_ctx.Change(Bip44Changes.CHAIN_EXT) # if you use "Solflare", remove this line and make a simple code modify and test
            priv_key_bytes = bip44_chg_ctx.PrivateKey().Raw().ToBytes()
            public_key_bytes = bip44_chg_ctx.PublicKey().RawCompressed().ToBytes()[1:]
            key_pair = priv_key_bytes+public_key_bytes

            return bip44_chg_ctx.PublicKey().ToAddress(), base58.Base58Encoder.Encode(key_pair)

```

## test
```
mnemonic="waste tattoo purse trumpet strong october danger desk custom settle become gaze"

print(f'mnemonic: {mnemonic}')
coin_types = {
    Bip44Coins.ETHEREUM: 'ethereum',
    Bip44Coins.SOLANA: 'solana',
    # Bip44Coins.TERRA: 'luna',
    # Bip44Coins.DASH: 'dash',
    # .....
    # also support other chain, such as file coin, eth classic, doge, dash, luna ....
    # example change coin_type as Bip44Coins.EOS, Bip44Coins.TERRA .....
   
}

# checking wallet address

for i in range(200):
    for coin_type in coin_types.keys():
        chain_name = coin_types[coin_type]
        bca = BlockChainAccount(mnemonic=mnemonic, coin_type=coin_type)
        address, pk = bca.get_address_pk(i)
        # print("wallet number",i)
        print(i,f'{chain_name} public: {address}, private: {pk}')
```
## result

```
mnemonic: waste tattoo purse trumpet strong october danger desk custom settle become gaze
0 ethereum public: 0x4BD56aE6630CD519A61607E026D3180E46C0f369, private: 76c5c40c4dffd83f5fcdbe12e6b9aea0bdf2fa3f85459112fe8fec88db50121e
0 solana public: EwNegFaq2A4Gxy3Y7jB5G8HazuHUWhsGTEU9ybDMyNMK, private: 44D34AgBFDagqxSkDBjb6vG2g3vaWsrfdPZ7i1UcLihZ3zPR9MbLRYAH5vWfd6StSmbWUzzJACbE59sGNTfM4Y8d
1 ethereum public: 0x4BD56aE6630CD519A61607E026D3180E46C0f369, private: 76c5c40c4dffd83f5fcdbe12e6b9aea0bdf2fa3f85459112fe8fec88db50121e
1 solana public: HSJhTtNYmUBqkxWRHcDxycnLpQckeucCVKhLQF9c2iJf, private: 36iHwEcmLSc8LFJmjGt2ZRXsVxpdy6RjwKDHBirL7pQZce3evKepTaFHt9kBiWxezXWbS3k8mMzJ7FrjhexHF6Rh
2 ethereum public: 0x4BD56aE6630CD519A61607E026D3180E46C0f369, private: 76c5c40c4dffd83f5fcdbe12e6b9aea0bdf2fa3f85459112fe8fec88db50121e
2 solana public: Ga5ytrZzRqAqNjcHQkN5SW1TQB5sx5dmKXV6pNFEeFCS, private: 2GXpQpcTD1n7NLymzubHCiAbrf7sr6t4vthhVEJKzmWenmWyzBfAZNeCYmwUUQbMLLsZkFt5mjQXjypwseCZw5aE
3 ethereum public: 0x4BD56aE6630CD519A61607E026D3180E46C0f369, private: 76c5c40c4dffd83f5fcdbe12e6b9aea0bdf2fa3f85459112fe8fec88db50121e
3 solana public: DbUHDiqnbanwaNwpWRjDXt3fVMR97S8EroKbfu4Midpr, private: qD32f8cqWAcicKKScWSk7yU3bApoMWSVBc5eX63Cn4KQE5Ryy1C28PpTUVDunP2dyTG8sX4SvqgWzXtV7CuNCEA
4 ethereum public: 0x4BD56aE6630CD519A61607E026D3180E46C0f369, private: 76c5c40c4dffd83f5fcdbe12e6b9aea0bdf2fa3f85459112fe8fec88db50121e
4 solana public: 7QUeJDQgzKqrRYseGFukgQNgH6aak5xbze7bNJiA6hWU, private: BXnkh8Tkwg1RS8DsJ9VDx2KtAiAFdo4gNzKA68RJGCLR7eygqnQG1ChRKqTrjsRGj7BMhiYNJx1ZJAZ6mcrRbY8
5 ethereum public: 0x4BD56aE6630CD519A61607E026D3180E46C0f369, private: 76c5c40c4dffd83f5fcdbe12e6b9aea0bdf2fa3f85459112fe8fec88db50121e
5 solana public: EZ4HG1MzY9RvNcEmbKjShE8tRJ4VyS28XfJBcUjZ9hN2, private: 4CgB6AhqSaAfYL3Dq5snTEAZtBr7wL7buPBBFb9RUHUb9oM8Atsg9sT185YJstgcLfKVpPrbEZhCEVyvrx4S3JHY
6 ethereum public: 0x4BD56aE6630CD519A61607E026D3180E46C0f369, private: 76c5c40c4dffd83f5fcdbe12e6b9aea0bdf2fa3f85459112fe8fec88db50121e
6 solana public: J38Qkj3LhoK3VJN4CJL34CciYNpFyH7osDxb885L9Jnk, private: bsmuCFzpjwunsVUXPY1Lt662QxMikgmubRXw3AwMXFxtZBQd9aJW8netj3tXHF4H9NWd98YqyAXJ8yXEyd7sj9A
7 ethereum public: 0x4BD56aE6630CD519A61607E026D3180E46C0f369, private: 76c5c40c4dffd83f5fcdbe12e6b9aea0bdf2fa3f85459112fe8fec88db50121e
7 solana public: 8syvCJPqAjChcrvihbeyCkYAipBwrSsWPhanPBsQdKrM, private: 516hrk1QrQocTosMLMhR6xTF3Ueb7crJeQxkuJNMYzYGHpT4SmkKQMx8WEtBka5ektsZNiSeAh1mLz5FLbXzBVCy
8 ethereum public: 0x4BD56aE6630CD519A61607E026D3180E46C0f369, private: 76c5c40c4dffd83f5fcdbe12e6b9aea0bdf2fa3f85459112fe8fec88db50121e
8 solana public: 8E9NmCwyV3hbzDeWgLr6KM47WxoQbzUQW9brXLQv1MJM, private: 4zL1W3XerrtqxCKA7Rw24KRSCEBVpGK2yCjfuQ7znenAwG6W7hX1HvmBuySk4NvRQfoHr6fgS2fJbmBvEyUibfGq
```

## import the mnemonic seed to phantom wallet , get result
![phantom](https://github.com/satisfywithmylife/phantom-py/assets/30144807/5eecbe32-3c6a-4b60-9cc1-504b8dc8b413)

## init account with python

```
pip install solana, web3
```

## test solana, eth

```
from solders.keypair import Keypair
from web3 import Web3
# solana
solana_private_key = '58hvcp5Lje9us9Q1QJdWV8aATcJKeHNZudyN9jWuiNYtRGFhJrH97Qe4ew8VmxLx5VCEYEuHGWRZuaFLr6A4euqR'
kp = Keypair().from_base58_string(solana_private_key) # private key
print(kp.pubkey()) # get "6RVympP2ZLR3T3KTSiqzCcBTvjRhT4UDCCt8AsbkYg2b", solana address
# eth
w3 = Web3(Web3.HTTPProvider("https://cloudflare-eth.com/v1/mainnet"))
eth_private_key = '4a68dfa8cb029fb5490cb36bb9c4c6523bada89134e40d2498cf83d7b4295cfb'
ac = w3.eth.account.from_key(eth_private_key) # private_key
print(ac.address) # get "0x58eE4fd2e1D2c970E1fAA8f888CFd1cA27BD4A28", eth address
```

# last but important!
1. test the result and compare it with main web wallet app(such as: metamask, mathwallet, trustwallet...) before you deposit crypto assets to the address
2. some wallet may get diffrent result, because it may use diffrent derive path to generate wallet
3. learn about hd-wallet principle by your self
