
# Image Template (for Azure Image Builder service)

This module deploys an Image Template that can be consumed by the Azure Image Builder service

## Resources

- Microsoft.VirtualMachineImages/imageTemplates
- Microsoft.VirtualMachineImages/imageTemplates/providers/locks

## Parameters

| Parameter Name | Type | Default Value | Possible values | Description |
| :-             | :-   | :-            | :-              | :-          |
| `imageTemplateName` | string | | | Required. Name of the Image Template to be built by the Azure Image Builder service.
| `location` | string | `[resourceGroup().location]` | | Optional. Location for all resources.
| `userMsiName` | string | | | Required. Name of the User Assigned Identity to be used to deploy Image Templates in Azure Image Builder.
| `buildTimeoutInMinutes` | int | 0 | 0-960 | Optional. Image build timeout in minutes. Allowed values: 0-960. 0 means the default 240 minutes
| `vmSize` | string | "Standard_D2s_v3" | | Optional. Specifies the size for the VM.
| `osDiskSizeGB` | int | 127 |  | Optional. Specifies the size of OS disk.
| `subnetId` | string | "" | | Optional. Resource Id of an already existing subnet, e.g. `/subscriptions/<subscriptionId>/resourceGroups/<resourceGroupName>/providers/Microsoft.Network/virtualNetworks/<vnetName>/subnets/<subnetName>`. If no value is provided, a new VNET will be created in the target Resource Group.
| `imageSource` | object | | Complex structure, see below. | Required. Image source definition in object format.
| `customizationSteps` | array | | | Required. Customization steps to be run when building the VM image.
| `managedImageName` | string | "" | | Optional. Name of the managed image that will be created in the AIB resourcegroup.
| `unManagedImageName` | string | "" | | Optional. Name of the unmanaged image that will be created in the AIB resourcegroup.
| `sigImageDefinitionId` | string | "" | | Optional. Resource Id of Shared Image Gallery to distribute image to, e.g.: `/subscriptions/<subscriptionID>/resourceGroups/<SIG resourcegroup>/providers/Microsoft.Compute/galleries/<SIG name>/images/<image definition>`
| `imageReplicationRegions` | string | "" | | Optional. List of the regions the image produced by this solution should be stored in the Shared Image Gallery. When left empty, the deployment's location will be taken as a default value.
| `managedImageName` | string | "" | | Optional. Name of the managed image that will be created in the AIB resourcegroup.
| `lockForDeletion` | bool | `true` | | Optional. Switch to lock the resource from deletion.
| `tags` | object | {} | Complex structure, see below. | Optional. Tags of the resource.
| `baseTime` | string | `utcNow('yyyy-MM-dd-HH-mm-ss')` | | Generated. Do not provide a value! This date value is used to generate a unique image template name.
| `cuaId` | string | "" | | Optional. Customer Usage Attribution id (GUID). This GUID must be previously registered

### Parameter Usage: `imageSource`

Tag names and tag values can be provided as needed. A tag can be left without a value.

#### Platform Image

```json
"source": {
    "type": "PlatformImage",
    "publisher": "MicrosoftWindowsDesktop",
    "offer": "Windows-10",
    "sku": "19h2-evd",
    "version": "latest"
}
```

#### Managed Image

```json
"source": {
    "type": "ManagedImage",
    "imageId": "/subscriptions/<subscriptionId>/resourceGroups/{destinationResourceGroupName}/providers/Microsoft.Compute/images/<imageName>"
}
```

#### Shared Image

```json
"source": {
    "type": "SharedImageVersion",
    "imageVersionID": "/subscriptions/<subscriptionId>/resourceGroups/<resourceGroup>/providers/Microsoft.Compute/galleries/<sharedImageGalleryName>/images/<imageDefinitionName/versions/<imageVersion>"
}
```

### Parameter Usage: `tags`

Tag names and tag values can be provided as needed. A tag can be left without a value.

```json
"tags": {
    "value": {
        "Environment": "Non-Prod",
        "Contact": "test.user@testcompany.com",
        "PurchaseOrder": "1234",
        "CostCenter": "7890",
        "ServiceName": "DeploymentValidation",
        "Role": "DeploymentValidation"
    }
}
```

## Outputs

| Output Name | Description |
| :-          | :-          |
| `imageTemplateName` | The Resource Id of the WVD Workspace. |
| `workspaceResourceGroup` | The name of the Resource Group the WVD Workspace was created in. |
| `workspaceName` | The Name of the Workspace. |

## Considerations

az network vnet subnet update --name aib --resource-group WVDCustomerEnvironment --vnet-name wvd-vnet --disable-private-link-service-network-policies true

## Additional resources

- [Preview: Create an Azure Image Builder template](https://docs.microsoft.com/en-us/azure/virtual-machines/linux/image-builder-json)
- [Use tags to organize your Azure resources](https://docs.microsoft.com/en-us/azure/azure-resource-manager/resource-group-using-tags)