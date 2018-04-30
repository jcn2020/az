
### Managing KeyVault
_---------_


-  connect azurerm account.  az login    -- get code to input to https://microsoft.com/devicelogin 
> `az login -u jnguyen@hotmail.com -p anythingGoes`
-  list account . az account list | jq ".[].name" --
-  Set to use correct account.  az account set --subscription ....
> ` az account set --subscription "7c3fae20-33c8-445e-8bbe-db192de9953a"`
-  List RG already there.  `az group list | jq ".[].name" `
- List locations availabe ` "az account list-locations | jq ".[].name"`
- Delete resource group `az group delete --name MyTempRG --nowait --yes`
- Create resource group `az group create --name MyTempRG --location "eastus" `

#### Register KeyVault resource provider
-  Register KV resource provider  
>  `az provider register -n Microsoft.KeyVault`
> `az provider show -n Microsoft.KeyVault`

#### Create a KeyVault
- Create a KeyVault 
> `az keyvault create -n "JNMyRGKeyVault" -g MyTempRG --location "eastus"`
- this will creaqte `https://JNMyRGKeyVault.vault.azure.net"`
- list KeyValut `az keyvault show -n JNMyRGKeyVault`
```   
{
  "id": "/subscriptions/7c3fae20-33c8-445e-8bbe-db192de9953a/resourceGroups/MyRG/providers/Microsoft.KeyVault/vaults/JNMyRGKeyVault",
  "location": "eastus2",
  "name": "JNMyRGKeyVault",
  "properties": {
    "accessPolicies": [
      {
        "applicationId": null,
        "objectId": "fc3db0d5-621a-47fb-ba0c-748cbac23cd7",
        "permissions": {
          "certificates": [
            "get",
            "list",
            "delete",
            "create",
            "import",
            "update",
            "managecontacts",
            "getissuers",
            "listissuers",
            "setissuers",
            "deleteissuers",
            "manageissuers",
            "recover"
          ],
          "keys": [
            "get",
            "create",
            "delete",
            "list",
            "update",
            "import",
            "backup",
            "restore",
            "recover"
          ],
          "secrets": [
            "get",
            "list",
            "set",
            "delete",
            "backup",
            "restore",
            "recover"
          ],
          "storage": [
            "get",
            "list",
            "delete",
            "set",
            "update",
            "regeneratekey",
            "setsas",
            "listsas",
            "getsas",
            "deletesas"
          ]
        },
        "tenantId": "841d6423-17a4-42e5-bf89-9f8217b8df46"
      }
    ],
    "additionalProperties": {
      "provisioningState": "Succeeded"
    },
    "createMode": null,
    "enableSoftDelete": null,
    "enabledForDeployment": false,
    "enabledForDiskEncryption": null,
    "enabledForTemplateDeployment": null,
    "sku": {
      "name": "standard"
    },
    "tenantId": "841d6423-17a4-42e5-bf89-9f8217b8df46",
    "vaultUri": "https://jnmyrgkeyvault.vault.azure.net/"
  },
  "resourceGroup": "MyRG",
  "tags": {},
  "type": "Microsoft.KeyVault/vaults"
}
```
Your Azure account is now authorized to perform any operations on this key vault. As yet, nobody else is.

##### Add a Key or secret to the key vault
* Ask Azure KeyVault to create a software-protected key
- `az keyvault key create --vault-name 'JNMyRGKeyVault' --name 'MyTopSecret' --protection software`
```
{
  "attributes": {
    "created": "2018-04-30T02:10:24+00:00",
    "enabled": true,
    "expires": null,
    "notBefore": null,
    "recoveryLevel": "Purgeable",
    "updated": "2018-04-30T02:10:24+00:00"
  },
  "key": {
    "d": null,
    "dp": null,
    "dq": null,
    "e": "AQAB",
    "k": null,
    "keyOps": [
      "encrypt",
      "decrypt",
      "sign",
      "verify",
      "wrapKey",
      "unwrapKey"
    ],
    "kid": "https://jnmyrgkeyvault.vault.azure.net/keys/MyTopSecret/f0f5b358de194369ae7556ffcb57c6f4",
    "kty": "RSA",
    "n": "3p23G/dwUBm/4dhcwOpd21tvcdbGk6QgQpsqgNNVn0Ou/tuwUco3ZMds/UGG4L5z+Flg4Bqiab7S86WSqk9DGXU0oPeUxwhEOPdNqCSayvtzp4epovzwXZL+m4CJv6ikExQVyBcuv8/VyglduXY82zomaDoMlLGX/qt7K49nSv7sfVwuKTc4cQP3frm021UOfhtJ2b3qHWPK6gkXc2JxiD1kBl2DTac0VbHcx3tRGiewTIx0mGq7zzGEXl8LdcYiq4CeCS76d/uTyCaJrq7W54rdjxT5dwPSvUpJhD/TWT1IJfkz3ZJx4cKe09KwXl4wSoINZYQjJkyXDcABMeP1JQ==",
    "p": null,
    "q": null,
    "qi": null,
    "t": null
  },
  "managed": null,
  "tags": null
}
```
* Create *.pem 
- `az keyvault key import --vault-name "JNMyRGKeyVault" --name "MyFirstKey" --pem-file "./softkey.pem" --pem-password "PaSSWORD" --protection software `

* Add secret to the vault 
> `az keyvault secret set --vault-name "JNMyRgKeyVault" --name "MyTopPassword" --value "MySeCREtPassWd*"`
```
{
  "attributes": {
    "created": "2018-04-30T02:10:24+00:00",
    "enabled": true,
    "expires": null,
    "notBefore": null,
    "recoveryLevel": "Purgeable",
    "updated": "2018-04-30T02:10:24+00:00"
  },
  "key": {
    "d": null,
    "dp": null,
    "dq": null,
    "e": "AQAB",
    "k": null,
    "keyOps": [
      "encrypt",
      "decrypt",
      "sign",
      "verify",
      "wrapKey",
      "unwrapKey"
    ],
    "kid": "https://jnmyrgkeyvault.vault.azure.net/keys/MyTopSecret/f0f5b358de194369ae7556ffcb57c6f4",
    "kty": "RSA",
    "n": "3p23G/dwUBm/4dhcwOpd21tvcdbGk6QgQpsqgNNVn0Ou/tuwUco3ZMds/UGG4L5z+Flg4Bqiab7S86WSqk9DGXU0oPeUxwhEOPdNqCSayvtzp4epovzwXZL+m4CJv6ikExQVyBcuv8/VyglduXY82zomaDoMlLGX/qt7K49nSv7sfVwuKTc4cQP3frm021UOfhtJ2b3qHWPK6gkXc2JxiD1kBl2DTac0VbHcx3tRGiewTIx0mGq7zzGEXl8LdcYiq4CeCS76d/uTyCaJrq7W54rdjxT5dwPSvUpJhD/TWT1IJfkz3ZJx4cKe09KwXl4wSoINZYQjJkyXDcABMeP1JQ==",
    "p": null,
    "q": null,
    "qi": null,
    "t": null
  },
  "managed": null,
  "tags": null
}
word" --value "MySeCREtPassWd*"az keyvault secret set --vault-name "JNMyRgKeyVault" --name "MyTopPass
{
  "attributes": {
    "created": "2018-04-30T02:21:03+00:00",
    "enabled": true,
    "expires": null,
    "notBefore": null,
    "recoveryLevel": "Purgeable",
    "updated": "2018-04-30T02:21:03+00:00"
  },
  "contentType": null,
  "id": "https://jnmyrgkeyvault.vault.azure.net/secrets/MyTopPassword/b033c1c9049a4a9c824bed732c986735",
  "kid": null,
  "managed": null,
  "tags": {
    "file-encoding": "utf-8"
  },
  "value": "MySeCREtPassWd*"
}
```
#### To View Password from KeyVault
* use this `https://jnmyrgkeyvault.vault.azure.net/secrets/MyTopPassword/b033c1c9049a4a9c824bed732c986735`


**az keyvault key list --vault-name "JNMyRGKeyVault"**
```[
  {
    "attributes": {
      "created": "2018-04-30T02:10:24+00:00",
      "enabled": true,
      "expires": null,
      "notBefore": null,
      "recoveryLevel": "Purgeable",
      "updated": "2018-04-30T02:10:24+00:00"
    },
    "kid": "https://jnmyrgkeyvault.vault.azure.net/keys/MyTopSecret",
    "managed": null,
    "tags": null
  }
]
```
 **az keyvault secret list --vault-name "JNMyRgKeyVault"**
 
```[
  {
    "attributes": {
      "created": "2018-04-30T02:21:03+00:00",
      "enabled": true,
      "expires": null,
      "notBefore": null,
      "recoveryLevel": "Purgeable",
      "updated": "2018-04-30T02:21:03+00:00"
    },
    "contentType": null,
    "id": "https://jnmyrgkeyvault.vault.azure.net/secrets/MyTopPassword",
    "managed": null,
    "tags": {
      "file-encoding": "utf-8"
    }
  }
]
```
