# Edoncoin API

## 1. General information.

Edoncoin works on port 13366. All API methods have prefix: */api*.

**Like:** */api/someMethod*

API methods response format - JSON.

All responses have *status* field and *success* field. If something is wrong, response contains error field.


|Field|Type|Description|
|-----|-----|-----|
|success|Boolean|Result of operation. If operation is successfuly completed,  value =  true, if operation fails, value is  = false.|
|status|String|Contains result operation. If operation is successfully completed, value = "OK", if operation fails, value =  error message.|


## 2. API Methods.

### NOTE: This is only a partial list, more methods will be added in the near future.


### 2. 1. Create account.

**Url:** */api/login*

**Method:** *POST*

**Description:** Creates a new account, or unlocks an existing one, using the provided secret.

**Parameters:**

|Field|Type|Description|
|-----|-----|-----|
|walletKey|String|Email + Password. From 10 characters. Example: test@gmail.compassword|

**Response:**

|Field|Type|Description|
|-----|-----|-----|
|success|Boolean|Result of success of operation.|
|status|String|"OK" or error message.|
|accountId|String|String. Account Id.|
|publicKey|String|Public key of account converted to hex format.|
|secretWord|String|Secret World of account converted to hex format.|
|accountTypes|Object|Contain all accounts (see example).|


**Example of accountTypes:**
```javascript

    "edonAccount": {
        "name": "edon",
        "id": "EDON4GLHgHeu7VfoAcLn5Qwgg3FnuWLy9AbruPHbD2EcPW25",
        "postfix": "EDON",
        "amount": 0,
        "unconfirmedAmount": 0
    }
### 2. 2. Get balance.

**Url:** */api/getAccountsByHash*

**Method:** *POST*

**Description:** Return balance of current account.

**Parameters:**

|Field|Type|Description|
|-----|-----|-----|
|secretWord|String|secretWord - received when login (2.1)|

**Response:**

|Field|Type|Description|
|-----|-----|-----|
|success|Boolean|Result of success of operation.|
|status|String|"OK" or error message.|
|accountTypes|Object|Contain all accounts (see in 2.1).|


### 2. 3. Send EDON.

**Url:** */api/createTransaction*

**Method:** *POST*

**Description:** Send EDON to another account. Fee will be calculated and applied automatically.

**Important:** Use json in body requests. And set Content-Type header to application/json.

**Parameters:**

|Field|Type|Description|
|-----|-----|-----|
|secretWord|String|Email + password. Example: test@gmail.compassword|
|amount|Float/Integer|Amount to be sent.|
|recipientId|String|Recipient address.|

**Response:**

|Field|Type|Description|
|-----|-----|-----|
|success|Boolean|Result of success of operation.|
|status|String|"OK" or error message.|
|transactionId|String|Transactions ID.|

### 2. 4. Transactions List.

**Url:** */api/getMyTransactions*

**Method:** *GET*

**Description:** Return last N transactions for the provided address.

**Parameters:**

|Field|Type|Description|
|-----|-----|-----|
|accountId|String|Address of transactions (sender, recipient).|
|limit|Integer|Limit of transactions. Optional parameter. Default value: 100.|


**Response:**

|Field|Type|Description|
|-----|-----|-----|
|success|Boolean|Result of success of operation.|
|status|String|"OK" or unique error code.|
|transactions|Array|JSON array of transactions.|


**Example of transaction:**
```javascript
{
	"deadline":"24",
	"senderPublicKey":"787b883b9633a02820931ff24b53ce5a2cee3ee2dd24334887becd3ab260603e",
	"recipientId":"EDON4w7md6oBE9U7aRFkhACngvG49e73Vg4SXT22EogUe2L8",
	"amount":234,
	"fee":0.819,
	"referencedTransactionId":"12386738521126635301",
	"type":0,"height":4,"blockId":"15164799571054225211",
	"signature":"581a787bd0211b37dd916c4dd0e4e8b3dbea2311ff003a5a311c20d04c989f00050296733138a54f0058805b3481bb80104eab81490d03f0dffbcf4cebf1b413",
	"timestamp":1450953432469,
	"attachment":null,
	"id":"9474303346278805986",
	"senderId":"EDONBkzGaFhMiJND2RqjxGDRqQo5iLReYvmLbcgkMsohB1Mm",
	"hash":"4a3ff44c3092dc550e9fa129b00b6058bc995b755802077d837b7cb2493e31e2",
	"confirmations":2,
	"message": "c0ced9ac0ed3f471334db60ec16e2bc2",
	"tbl":"transaction",
	"_id":"YWVA7hN1I8eudywl"
}
```


### 2. 6. Get specific transaction details

**Url:** */api/getTransactionById?transactionId=TXT_ID*

**Method:** *GET*

**Description:** Return the details of the transaction specified by the transaction id.

**Parameters:**

|Field|Type|Description|
|-----|-----|-----|
|transactionId|String|ID of the transaction.|


**Response:**

|Field|Type|Description|
|-----|-----|-----|
|success|Boolean|Result of success of operation.|
|status|String|"OK" or unique error code.|
|transaction|Object|JSON object of transaction.|


**Example of transaction:**
```javascript
{
	"deadline":"24",
	"senderPublicKey":"787b883b9633a02820931ff24b53ce5a2cee3ee2dd24334887becd3ab260603e",
	"recipientId":"EDON8Svi52EpHF7VVFzgBQ2tNHrQJKcsWBDVhkpyGkg51xES",
	"amount":0.0001,
	"fee":0.00001,
	"referencedTransactionId":"9474303346278805986",
	"type":0,
	"height":5,
	"blockId":"2410935853770602461",
	"signature":"bc2d899c148ebc9b29de90fc597b1ca8a12b2f2cfdfca1bc2a7642e4f31eac08cb5f2c240d7e87da2b06602d694b39c4fa636041702d46c91e22cfe99988a638",
	"timestamp":1451045061656,
	"attachment":null,
	"id":"6773761851285509835",
	"senderId":"EDONBkzGaFhMiJND2RqjxGDRqQo5iLReYvmLbcgkMsohB1Mm",
	"hash":"5f607244df4adc1eed7c1c922e178b180cf2f9e6d3ebe3345e013c83caf716cb",
	"confirmations":2,
	"message":"",
	"tbl":"transaction",
	"_id":"Mr5BCn8fAIJhreeA"
}
```

### 2. 7. Get account list

**Url:** */api/getAllAccounts*

**Method:** *GET*

**Description:** Return the list of accounts witch was made at least one transaction or login into client.

**Parameters:**

none

**Response:**

|Field|Type|Description|
|-----|-----|-----|
|success|Boolean|Result of success of operation.|
|status|String|"OK" or unique error code.|
|accountList|Object|JSON object list of accounts.|

**Example of account list:**
```javascript
{
"EDON4w7md6oBE9U7aRFkhACngvG49e73Vg4SXT22EogUe2L8":
	{
	"id":"EDON4w7md6oBE9U7aRFkhACngvG49e73Vg4SXT22EogUe2L8",
	"height":0,
	"publicKey":[25,194,214,39,141,72,237,81,118,129,147,58,244,253,191,207,238,15,166,227,20,23,109,64,159,176,65,247,89,48,110,122],
	"keyHeight":-1,
	"balance":301.02299,
	"unconfirmedBalance":301.02299,
	"guaranteedBalances":{},
	"assetBalances":{},
	"unconfirmedAssetBalances":{},
	"accountTypes":{}
}
	...
}
```

### 2. 8. Get account by ID

**Url:** */api/getAccountById*

**Method:** *GET*

**Description:** Return the account info by ID.

**Parameters:**

|Field|Type|Description|
|-----|-----|-----|
|accountId|String|ID of the account (account number).|

**Response:**

|Field|Type|Description|
|-----|-----|-----|
|success|Boolean|Result of success of operation.|
|status|String|"OK" or unique error code.|
|account|Object|JSON object of account.|

**Example of account data:**
```javascript
{
	"accountId":"EDON4GLHgHeu7VfoAcLn5Qwgg3FnuWLy9AbruPHbD2EcPW25",
		"accountTypes":
		{
			"edonAccount":{"name":"edon","id":"EDON4GLHgHeu7VfoAcLn5Qwgg3FnuWLy9AbruPHbD2EcPW25EDON","postfix":"EDON","amount":13605,"unconfirmedAmount":13605},"dollarAccount":{"name":"dollar","id":"EDON4GLHgHeu7VfoAcLn5Qwgg3FnuWLy9AbruPHbD2EcPW25USD","postfix":"USD","amount":0,"unconfirmedAmount":0},"euroAccount":{"name":"euro","id":"EDON4GLHgHeu7VfoAcLn5Qwgg3FnuWLy9AbruPHbD2EcPW25EUR","postfix":"EUR","amount":0,"unconfirmedAmount":0},"btcAccount":{"name":"btc","id":"175tWpb8K1S7NmH4Zx6rewF9WQrcZv245W","postfix":"BTC","amount":0,"unconfirmedAmount":0}
		},
		"recentTransactions":
			[
				{"deadline":"24","senderPublicKey":"787b883b9633a02820931ff24b53ce5a2cee3ee2dd24334887becd3ab260603e","recipientId":"EDON9gJoeAYQKPHEiFYk1aVRqwZAgMy7PctM8tpjY1jHeACu","amount":100,"fee":0.35,"referencedTransactionId":"5375121523786056773","type":0,"height":272,"blockId":"1887042855860437064","signature":"0c26ff1195330ca1b564dc468a8c405c9c590f17abb3baee99e989cefb08c20990c84ec0b27bfe069eaefdda2b4fbd913c169c6cf229ff0f6070755dd4e30c58","timestamp":1457288361447,"attachment":null,"id":"17189053270129408464","senderId":"EDONBkzGaFhMiJND2RqjxGDRqQo5iLReYvmLbcgkMsohB1Mm","hash":"14eebc0ea927b3aefe9115d647f800d062a634ee648c20f5ee8bc8e0af5479d0","confirmations":1,"message":"","tbl":"transaction","_id":"G1rFD7lWhog2Qgxz"}
			]
}
```

### 2. 9. Get peer list

**Url:** */api/getAllPeers*

**Method:** *GET*

**Description:** Return the list of peers connected to current client.

**Parameters:**

none

**Response:**

|Field|Type|Description|
|-----|-----|-----|
|success|Boolean|Result of success of operation.|
|status|String|"OK" or unique error code.|
|peers|Object|JSON object list of peers.|

**Example of peer list:**
```javascript
[
	{
		"port":13367,
		"host":"127.0.0.1",
		"uploadBytes":861,
		"downloadBytes":4312,
		"services":null,
		"MAX_LOST_CONNECTION":10,
		"lostConnection":0,
		"status":"check",
		"versionClient":1,
		"platform":"linux",
		"node_version":"v0.10.25",
		"accountId":"EDONA8vG2f2f45szLEM9pNnTHWQnBkrQU3tKKSyG8HjmVxhT"
	},
	...
]
```
