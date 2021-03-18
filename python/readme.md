proofofpersonhood.com SDKs > python

to run, from this folder:

```
pip3 install web3==4.5.0
python3

```

then within python

```
# imports
from pop_api import get_personhoodscore
import web3

#set up
network = 'rinkeby'
infura_api_key = 'TODO'
w3 = web3.Web3(web3.Web3.HTTPProvider(f'https://{network}.infura.io/v3/{infura_api_key}'))

# make the call
addresses = [
    '0x00De4B13153673BCAE2616b67bf822500d325Fc3',
    '0xDDF369C3bf18b1B12EA295d597B943b955eF4671',
]
for address in addresses:
    score = get_personhoodscore(w3, network, address)
    print(f"{address} personhood score is {score}")


```
