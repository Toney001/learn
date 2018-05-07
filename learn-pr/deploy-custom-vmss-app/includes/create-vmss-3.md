When you created your scale set with the Azure CLI 2.0 in the preceding step, it also created an Azure Load Balancer. The VM instances created from your Packer image were attached to this load balancer. The Azure Load Balancer distributes traffic to the VM instances in a scale set based on defined rules.

To allow traffic to reach your scale set and that verify that the NGINX web server works correctly, create a load balancer rule with [az network lb rule create](/cli/azure/network/lb/rule#create). The following example creates a rule named *myLoadBalancerRuleWeb* that allows traffic on *TCP* port *80*:

```azurecli
az network lb rule create \
  --resource-group myResourceGroup \
  --name myLoadBalancerRuleWeb \
  --lb-name myScaleSetLB \
  --backend-pool-name myScaleSetLBBEPool \
  --backend-port 80 \
  --frontend-ip-name loadBalancerFrontEnd \
  --frontend-port 80 \
  --protocol tcp
```