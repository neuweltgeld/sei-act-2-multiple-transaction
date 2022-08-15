# Place multiple Long/Short orders in one transaction

Sei uzerinde yer alan bu gorevi yapmak icin asagidaki komutlari takip etmeniz yeterli. Icerik Turkce'ye ceviridir.[^1]

### 1. Tx komutu olusturmak
Asagida yer alan komutlarda degistirmeniz gereken ve opsiyonel olan 2 kisim var.
Degistirmeniz gerekenler

``creator = "sei-adresiniz" ``
ve
``
account = "sei-adresiniz"
``

Sei-adresiniz yazan yerlere kendi cuzdan adresinizi yazin.

Opsiyonel olan kisimlar ise 

``price = "specify your price" , quantity = "specify your quantity", orderType ="LIMIT or MARKET", positionDirection = " LONG or SHORT"``

Kendi isteginize gore degistirebilir ya da sabit birakabilirsiniz.


````
echo '{
  "body": {
    "messages": [
      {
        "@type": "/seiprotocol.seichain.dex.MsgPlaceOrders",
        "creator": "sei-adresiniz",
        "orders": [
          {
            "id": "0",
            "status": "PLACED",
            "account": "sei-adresiniz",
            "contractAddr": "sei14hj2tavq8fpesdwxxcu44rty3hh90vhujrvcmstl4zr3txmfvw9sh9m79m",
            "price": "1.000000000000000000",
            "quantity": "0.000010000000000000",
            "priceDenom": "USDC",
            "assetDenom": "ATOM",
            "orderType": "LIMIT",
            "positionDirection": "LONG",
            "data": "{\"position_effect\":\"Open\",\"leverage\":\"1\"}",
            "statusDescription": ""
          }
        ],
        "contractAddr": "sei14hj2tavq8fpesdwxxcu44rty3hh90vhujrvcmstl4zr3txmfvw9sh9m79m",
        "funds": [
          {
            "denom": "uusdc",
            "amount": "10"
          }
        ],
        "autoCalculateDeposit": false
      },
      {
        "@type": "/seiprotocol.seichain.dex.MsgPlaceOrders",
        "creator": "sei-adresiniz",
        "orders": [
          {
            "id": "0",
            "status": "PLACED",
            "account": "sei-adresiniz",
            "contractAddr": "sei14hj2tavq8fpesdwxxcu44rty3hh90vhujrvcmstl4zr3txmfvw9sh9m79m",
            "price": "1.000000000000000000",
            "quantity": "0.000010000000000000",
            "priceDenom": "USDC",
            "assetDenom": "ATOM",
            "orderType": "LIMIT",
            "positionDirection": "LONG",
            "data": "{\"position_effect\":\"Open\",\"leverage\":\"1\"}",
            "statusDescription": ""
          }
        ],
        "contractAddr": "sei14hj2tavq8fpesdwxxcu44rty3hh90vhujrvcmstl4zr3txmfvw9sh9m79m",
        "funds": [
          {
            "denom": "uusdc",
            "amount": "10"
          }
        ],
        "autoCalculateDeposit": false
      }
    ],
    "memo": "",
    "timeout_height": "0",
    "extension_options": [],
    "non_critical_extension_options": []
  },
  "auth_info": {
    "signer_infos": [],
    "fee": {
      "amount": [
        {
          "denom": "usei",
          "amount": "0"
        }
      ],
      "gas_limit": "0",
      "payer": "",
      "granter": ""
    }
  },
  "signatures": []
}' > $HOME/gen_tx.json
````

### 2.Tx imzalama asamasi
Asagidaki komutlara ``sei-adresiniz`` yazan yere cuzdan adresi gelecek

````
ACC=$(seid q account sei-adresiniz -o json | jq -r .account_number)
````
````
seq=$(seid q account sei-adresiniz -o json | jq -r .sequence)
````

````
seid tx sign $HOME/gen_tx.json -s $seq -a $ACC --offline \
--from sei-adresiniz --chain-id atlantic-1 \
--output-document $HOME/txs.json
````

### 3.Tx gorelim

````
seid tx broadcast $HOME/txs.json
````

Basarili sonuc alirsaniz soyle bir cikti elde edersiniz

````
code: 0
codespace: ""
data: ""
events: []
gas_used: "0"
gas_wanted: "0"
height: "0"
info: ""
logs: []
raw_log: '[]'
timestamp: ""
tx: null
txhash: 5BA641A155C054BB51A709AA09D703FF98FBEBF214415A186788B78B048DC29C
````

Txhash'inizi explorerdan kontrol ederek success ya da fail mesajini gorebilirsiniz. Success olduysa islem tamamdir.

![image](https://user-images.githubusercontent.com/101174090/184542304-a70d00ec-b2e2-4390-9b58-efa4236a8e49.png)



[^1]: *https://craving-for-knowledge.gitbook.io/craving_for_knowledge/proekty/sei/act-2-missions/place-multiple-orders-in-one-transaction*
