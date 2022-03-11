### Download Deployer VM Packages in storage account

- Create a new storage account where the SAP bits will reside.
- Create a container "sapbits" in the storage account.
- Create a new Ubuntu (20.04) VM in Azure.
- Log into the VM create in above step.
- Install az cli. Steps mentioned at
  https://docs.microsoft.com/en-us/cli/azure/install-azure-cli-linux?pivots=apt

- Login into azure using command
```
az login
```

- Download shell script to download deployer VM packages using below command:
```
wget "https://raw.githubusercontent.com/ajaygit158/DeployerVmPackages/master/DownloadDeployerVMPackages.sh"
```

- Change the permissions of the above script file using
```
chmod +x DownloadDeployerVMPackages.sh
```

- Run the script
```
./DownloadDeployerVMPackages.sh
```
- When asked for do you have a storage account, enter 'y'.
- When asked for SAP storage account base path, enter the container path (eg: https://sapbitsstorageaccount.blob.core.windows.net/sapbits). You can get this from portal by going to the storage account created in step 1 --> container (sapbits) --> Properties --> URL.

![](https://github.com/ajaygit158/DeployerVmPackages/raw/master/SapBitsContainerUrl.jpg)
- When asked for access key, Go to storage account --> Access Keys --> Show Keys --> Copy Key 1

![](https://github.com/ajaygit158/DeployerVmPackages/raw/master/StorageAccountAccessKey.jpg)

- The deployer VM packages will now be there in storage account

![](https://github.com/ajaygit158/DeployerVmPackages/raw/master/DeployerVmPackagesFolder.jpg)


# Download SAP Bits

- Install ansible on the ubuntu VM
```
sudo pip3 install ansible==2.9.27
```

- Git clone the below repository
```
git clone https://github.com/Azure/sap-automation.git
```

- Run the playbook_bom_downloader ansible script using below command
```
ansible-playbook ./sap-automation/deploy/ansible/playbook_bom_downloader.yaml -e "bom_base_name=S41909SPS03_v0010ms" -e "kv_name=abcd" -e "deployer_kv_name=abcd" -e "s_user=<username>" -e "s_password=<password>" -e "sapbits_access_key=<storageAccountAccessKey>" -e "sapbits_location_base_path=<ContainerBasePath>" -e "sapbits_sas_token=<containerSasToken>"
```

##### Replace the values in angular brackets as per below:
- **kv_name** --> any non null value
- **deployer_kv_name** --> any non null value
- **s_user** --> Your S Username
- **s_password** --> Password corresponding to S username
- **sapbits_access_key** --> you can get this from the storage account --> Access Keys –> Key 1
- **sapbits_sas_token** --> you can get this from the storage account --> container(sapbits) --> Shared Access token –> Generate SAS Token and URL --> Blob SAS Token
- **sapbits_location_base_path** --> storage account --> container (sapbits) --> Properties --> URL


The container will look as below if everything succeeds.

![](https://github.com/ajaygit158/DeployerVmPackages/raw/master/SapBitsContainerUrl.jpg)
