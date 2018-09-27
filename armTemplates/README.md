# Deploying Image Builder Templates with Azure Resource Manager (ARM) 
This repo contains deployment templates and parameters files - this will allow you to create an ImageTemplate immediately using ARM. The beauty of these examples, they are heavily parameterized, so you just need to drop in your own details, then begin image building! 

There are two files required for creating the Image Template, deployment template and parameters file, you can recognise the pair, as the only difference in the file name is 'deploy' and 'param', for example:
* rhel_iso_image_params_mdi.json - parameters file
* rhel_iso_image_deploy_mdi.json - associated deployment template file

Format:
<source>_<template_type>_<distribution_target>.json

* source e.g. iso or azplatform (azure marketplace image (Ubuntu))
* template_type e.g. deploy or params, deploy is the ARM deployment template, the params represents the parameter file, which you will need to apply your own settings.
* distribution_target e.g. mdi (managed disk image) or sig (shared image gallery)
* All examples include at least 1 customization


e.g.
* rhel_iso_image_deploy_sigmdi.json - RHEL 73 ISO, ARM deployment template, distributing to Shared Image Gallery (SIG), and MDI (Managed Image)
* azplatform_image_params_sig.json - Azure Platform Image, ARM parameters file, distributing to Shared Image Gallery (SIG).

## How to deploy the templates using ARM
Once you have copied the parameters file locally and populated, note the Image Template Name, you will need this to invoke the image build.

```bash
# template path must be the RAW git code!

declare resourceGroupName=""
declare deploymentName=""
declare templateFilePath="https://raw.githubusercontent.com/danielsollondon/azvmimagebuilder/master/armTemplates/azplatform_image_deploy_sigmdi.json"
declare parametersFilePath="pathToLocalParamsFile.json"

az group deployment create --name $deploymentName --resource-group $resourceGroupName --template-uri $templateFilePath --parameters $parametersFilePath 
```

### How to create image artifact
```bash
az resource invoke-action \
     --resource-group ibdemo \
     --resource-type  Microsoft.VirtualMachineImages/imageTemplates \
     -n <imageTemplateName> \
     --action Run 
```

### Check image artifact status
```bash
az resource show \
    --resource-group ibdemo \
    --resource-type Microsoft.VirtualMachineImages/imageTemplates \
    -n <imageTemplateName>
```
### Create a VM

#### Managed Image Image
```bash
az vm create \
  --resource-group <vmResourceGroup> \
  --name <vmName> \
  --location <region - must be the same as image> \
  --admin-username <userName> \
  --image /subscriptions/<subid>/resourceGroups/<imageResourceGroup>/providers/Microsoft.Compute/images/<managedImagename> \
  --ssh-key-value /../.../.pub    
```
#### Shared Image Gallery Image
```bash
az vm create \
  --resource-group <vmResourceGroup> \
  --name <vmName> \
  --location <region - must be the same as image> \
  --admin-username <userName> \
  --image /subscriptions/<subid>/resourceGroups/<imageResourceGroup>/providers/Microsoft.Compute/galleries/<imageGalName>/images/<ImageDefintionName>/versions/<ImageDefintionVersion> \
  --ssh-key-value /../.../.pub   
```