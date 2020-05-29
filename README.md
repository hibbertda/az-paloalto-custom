# az-PaloAlto-Custom

This ARM template will deploy a single Palo Alto VMSeries network security applicance to Azure connected to an existing Virtual Network (VNet) and Subnets. 


## Terms and Coniditions

If you haven't deployed a PaloAlto virtual appliance in your Azure subscriptions before, you will need to accept the PaloAlto terms of service. The template deployment process cannot prompt to accept terms, and it will cause the deployment to fail.

Use '**Get-AzMarketplaceTerms**' PowerShell cmdlet to check the current status. 

```powershell
    Get-AzMarketplaceTerms `
        -Publisher paloaltonetworks `
        -Product vmseries1 `
        -Name byol

    ###

    Publisher         : paloaltonetworks
    Product           : vmseries1
    Plan              : byol
                        Q6A
    Accepted          : False

```
### Accept Terms

The following command will accept the PaloAlto terms and conditions in the current subscription.

```powershell
    Get-AzMarketplaceTerms `
        -Publisher paloaltonetworks `
        -Product vmseries1 `
        -Name byol `
        | Set-AzMarketplaceTerms -Accept

    ###

    Publisher         : paloaltonetworks
    Product           : vmseries1
    Plan              : byol
                        Q6A
    Accepted          : True

```

## Parameters

### vnet

The **vnet** parameters is an array of required configuration inforamtion for the target Virtual Network. 

|Object Parameter|Description|
|---|---|
|name|Virtual Network Name|
|ipv4prefix|The IPv4 address prefix for the network in CIDR notation|
|resourceGroup|Resource Group name where target VNet is located


```json
    "vnet": {
        "value": {
            "name": "VALUE",
            "ipv4prefix": "VALUE",
            "resourceGroup": "VALUE"
        }
    },
```

### subnet

**subets** is an array of all subnets the virtual PaloAlto will be connected to. A Network Interface (NIC) will be created and attached to the VM for each of the subnets listed. 

|Array Parameter|Description|
|---|---|
|name|Subnet name|
|ipv4prefix|The IPv4 address prefix for the subnet in CIDR notation|


```json
    "subnet": {
        "value": [
            {
                "name": "VALUE",
                "ipv4Prefix": "VALUE",
                "primary": "[true/false]"
            },
            {
                "name": "VALUE",
                "ipv4Prefix": "VALUE",
                "primary": "[true/false]"
            },
            {
                "name": "VALUE",
                "ipv4Prefix": "VALUE",
                "primary": "[true/false]"
            }                                
        ]
    },
```

### osConfig

**osConfig** are configuration options for the OS configuration

|Parameter|Description|
|---|---|
|adminUserName|Local administrator account name. Will be used at the default admin user in PanOS

```json
    "osConfig": {
        "value": {
            "adminUserName": "VALUE"
        }
    },
```

### osConfigPassword

**osConfigPassword** is a **Secure String** for the default administrator account password. 

```json 
    "osConfigPassword": {
        "value": "VALUE"
    }
```