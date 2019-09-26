# Azure VDC Networking lab

Artefacts for the VDC lab.

The lab itself is found in the <https://github.com/azurecitadel/azurecitadel.github.io> repository, in the workshops/vdc folder.

The Jekyll rendered Citadel pages can be found at <https://aka.ms/citadel/vdc>.

-----------------
az login

az account show --output jsonc

az account list --output table

az provider register --namespace Microsoft.Insights

az provider show --namespace Microsoft.Insights --query registrationState --output tsv

for rg in Hub Spoke1 Spoke2 OnPrem NVA

do az group create --location westeurope --name VDC-$rg

done

----Accept the Cisco CSR 1000v Marketplace terms

for urn in $(az vm image list --all --publisher cisco --offer cisco-csr-1000v --sku 16_6 --query '[].urn' --output tsv)

do az vm image accept-terms --urn $urn

done

az group deployment list -g VDC-Hub -o table

-----Deploy the ARM template  around 45 minutes
master=https://raw.githubusercontent.com/todorovc/vdc/master/DeployVDCwithNVA.json

az group deployment create --name VDC-Create --resource-group VDC-Hub --template-uri $master --verbose



--Delete

for rg in Hub Spoke1 Spoke2 OnPrem NVA

do az group delete --yes --no-wait --name VDC-$rg

done


--check

az group deployment list -g VDC-Hub -o table
