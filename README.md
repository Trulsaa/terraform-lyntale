# Dine første steg med terraform i Azure

1. Installer [terraform](https://www.terraform.io/downloads.html) og [az](https://docs.microsoft.com/en-us/cli/azure/install-azure-cli?view=azure-cli-latest) lokalt
2. Kjør `az login`
3. Logg inn i [portal.azure.com](https://portal.azure.com)
4. Gå til [https://www.terraform.io/docs/providers/azurerm/index.html](https://www.terraform.io/docs/providers/azurerm/index.html)
5. Lag `main.tf`
6. Legg inn en provider

```
provider "azurerm" {
  subscription_id = ""
  tenant_id       = ""
  version         = "=2.20.0"
  features {}
}
```

7. Legg inn resource group

```
resource "azurerm_resource_group" "rg" {
  name     = "alvtime-lyntale"
  location = "West Europe"
}
```

8. Legg inn app service plan

```
resource "azurerm_app_service_plan" "asp" {
  name                = "${azurerm_resource_group.rg.name}-asp"
  location            = azurerm_resource_group.rg.location
  resource_group_name = azurerm_resource_group.rg.name
  kind                = "Linux"
  reserved            = true

  sku {
    tier = "Shared"
    size = "B1"
  }
}
```

9. Legg inn app service

```
resource "azurerm_app_service" "as" {
  name                = "${azurerm_resource_group.rg.name}-as"
  location            = azurerm_resource_group.rg.location
  resource_group_name = azurerm_resource_group.rg.name
  app_service_plan_id = azurerm_app_service_plan.asp.id

  site_config {
    app_command_line = ""
    linux_fx_version = "DOCKER|alvnoas/alvtime-vue-pwa-dev:latest"
  }
}
```

10. Kjør `terraform init`
11. Kjør `terraform plan`
12. Kjør `terraform apply`
13. Gå til app servicen i [portal.azure.com](portal.azure.com)
14. Kjør `terraform destroy`
