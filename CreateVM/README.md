
### Create a VM
_---------_


-  connect azurerm account.  az login    -- get code to input to https://microsoft.com/devicelogin
-  list account . az account list | jq ".[].name" --
-  Set to use correct account.  az account set --subscription ....
> ` az account set --subscription f359300f-9311-4c20-bae2-3e3e1ac7cecd`
-  List RG already there.  `az group list | jq ".[].name" `
- Delete resource group `az group delete --name MyTempRG --nowait --yes`
- Create resource group `az group create --name MyTempRG --location "eastus" `
-  Create a VM `az vm create --resource-group MyTempRG --name "MyVM" --image UbuntuLTS --generate-ssh-keys --verbose`
- Delete a vm `az vm delete --name MyVM --resource-group MyTempRG --verbose`
- Delete a resource group `az group delete --name MyTempRG --verbose`
- logout `az account logout`
```
[
  {
    "cloudName": "AzureCloud",
    "id": "0155ae7b-d9aa-4d1f-be91-48467dbf029e",
    "isDefault": true,
    "name": "s00224csubnpcdnp01",
    "state": "Enabled",
    "tenantId": "ee69be27-d938-4eb5-8711-c5e69ca43718",
    "user": {
      "name": "jnguyen@starbucks.com",
      "type": "user"
    }
  },
  {
    "cloudName": "AzureCloud",
    "id": "f359300f-9311-4c20-bae2-3e3e1ac7cecd",
    "isDefault": false,
    "name": "s00224dsubsb00lgcy",
    "state": "Enabled",
    "tenantId": "ee69be27-d938-4eb5-8711-c5e69ca43718",
    "user": {
      "name": "jnguyen@starbucks.com",
      "type": "user"
    }
  }
]
```
