

# Buckets Azure

`az storage account list --output table | awk '{print $2}' | xargs -I {} az storage container list --account-name {} --output table`

# Azure - subdominios 

`az webapp list --query "[].{name:name, hostNames:hostNames}" -o table`
