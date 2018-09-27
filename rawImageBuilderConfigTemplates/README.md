# Raw Image Builder Templates
these are examples of Azure VM Image Builder Template, you can use these when submitting an Image Template directly to the Azure Resource Provider, and they are also good for understanding basic templates.

Format:
'<os_major_minor>_<source>_<number of customizations>_<distribution_target>.json'

* os_major_minor e.g. rhel73
* source e.g. iso or mp (azure marketplace image)
* number of customizations e.g. 1customize
* distribution_target e.g. mdi (managed disk image) or sig (shared image gallery)

e.g.
* rhel73_iso_1customize_sigmdi
* ubuntu1804_mp_1customize_mdi

## How to deploy the templates using ARM
```bash
az resource create --resource-group <rgname> --properties @/.../templateName.json --is-full-object --resource-type Microsoft.VirtualMachineImages/imageTemplates -n <imageTemplateName> 
```

### How to create image artifact
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