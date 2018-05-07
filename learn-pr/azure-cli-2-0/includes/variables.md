 Now that you have the NIC ID, run `az network nic show` to get its information.

```azurecli
az network nic show --ids $NIC_ID -g TutorialResources
```

This command shows all of the information for the network interface of the VM. This data includes DNS settings, IP information, security settings, and the MAC address. Right now the goal is to obtain the public IP address and subnet object IDs.

```azurecli
az network nic show --ids $NIC_ID \
  -g TutorialResources \
  --query '{IP:ipConfigurations[].publicIpAddress.id, Subnet:ipConfigurations[].subnet.id}' 
```

```json
{
  "IP": [
    "/subscriptions/.../resourceGroups/TutorialResources/providers/Microsoft.Network/publicIPAddresses/TutorialVM1PublicIP"
  ],
  "Subnet": [
    "/subscriptions/.../resourceGroups/TutorialResources/providers/Microsoft.Network/virtualNetworks/TutorialVM1VNET/subnets/TutorialVM1Subnet"
  ]
}
```

This command displays a JSON object that has custom keys ('IP' and 'Subnet') for the extracted values. While this style of output might not be useful for command-line tools, it helps with human readability and can be used with custom scripts.

In order to use command-line tools, change the command to remove the custom JSON keys and output as `tsv`. This style of output can be processed by the shell `read` command to load results into multiple variables. Since two values on separate lines are displayed, the `read` command delimiter must be set to the empty string rather than the default of non-newline whitespace.

```bash
read -d '' IP_ID SUBNET_ID <<< $(az network nic show \
  --ids $NIC_ID -g TutorialResources \
  --query '[ipConfigurations[].publicIpAddress.id, ipConfigurations[].subnet.id]' \
  -o tsv)
```

You won't use the subnet ID right away, but it should be stored now to avoid having to perform a second lookup later. For now, use the public IP object ID to look up the public IP address and store it in a shell variable.

```bash
VM1_IP_ADDR=$(az network public-ip show --ids $IP_ID \
  -g TutorialResources \
  --query ipAddress \
  -o tsv)
```

Now you have the IP address of the VM stored in a shell variable. Go ahead and check that it is the same value that you used to initially connect to the VM.

```bash
echo $VM1_IP_ADDR
```
