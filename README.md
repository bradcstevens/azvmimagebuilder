# Azure VM Image Builder Template Repo
Please find test templates for Azure VM Image Builder.

Format:
<os_major_minor>_<source>_<number of customizations>_<distribution_target>

* os_major_minor e.g. rhel73
* source e.g. iso or mp (azure marketplace image)
* number of customizations e.g. 1customize
* distribution_target e.g. mdi (managed disk image) or sig (shared image gallery)

e.g.
rhel73_iso_1customize_sigmdi
ubuntu1804_mp_1customize_mdi

## How to deploy the templates using ARM
```bash
az resource create --resource-group <rgname> --properties @/.../templateName.json --is-full-object --resource-type Microsoft.VirtualMachineImages/imageTemplates -n <templateName> --debug
```

### How to create image artifact
```bash
az resource invoke-action --resource-group <rgname> --resource-type  Microsoft.VirtualMachineImages/imageTemplates -n <templateName> --action Run --debug
```