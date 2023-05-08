# Azure - buckets

`az storage account list --output table | awk '{print $2}' | xargs -I {} az storage container list --account-name {} --output table`

# Azure - subdomínios 

`az webapp list --query "[].{name:name, hostNames:hostNames}" -o table`


# AWS - buckets

```
aws s3api list-buckets --profile mfa
```


# AWS - subdomínios

```
#!/bin/bash

# executar no terminal: bash subdominios-aws.sh

# obtem uma lista de todas as zonas
hosted_zones=$(aws route53 list-hosted-zones --profile mfa --query "HostedZones[].Id" --output text)

# percorre cada zona e exporta seus subdomínios
for zone_id in $hosted_zones; do
    zone_name=$(aws route53 get-hosted-zone --id $zone_id --profile mfa --query "HostedZone.Name" --output text)
    echo "Exportando subdominios da zona: $zone_name ($zone_id)"
    aws route53 list-resource-record-sets --hosted-zone-id $zone_id --profile mfa --query "ResourceRecordSets[?Type == 'CNAME'].Name" --output text > "$zone_name-subdomains.txt"
done
```

# GCP - buckets

```
for projeto in $(gcloud projects list --format="value(projectId)"); do echo "Projeto: $projeto"; gcloud config set project $projeto --quiet && gsutil ls -p $projeto | awk -F/ '{print $3}'; done
```

# GCP - subdomínios

```
for project in $(gcloud projects list --format="value(projectId)"); do gcloud compute instances list --project $project --format="value(networkInterfaces[0].accessConfigs[0].natIP)" | sed "s/^/$project /" >> external_ips.txt; done
```
