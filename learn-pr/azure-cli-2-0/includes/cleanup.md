Now that the tutorial is complete, it's time to clean up the created resources. You can delete individual resources with the `delete` command, but the safest way to remove all resources in a resource group is with `group delete`.

```azurecli
az group delete --name TutorialResources --no-wait
```

This command deletes the resources created during the tutorial, and is guaranteed to deallocate them in the correct order. The `--no-wait` parameter keeps the CLI from blocking while the deletion takes place. If you want to wait until the deletion is complete or watch it progress, use the `group wait` command.

```azurecli
az group wait --name TutorialResources --deleted
```
