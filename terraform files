provider "azurerm" {
  version = "=2.54.0"
  features {}
}

resource "azurerm_resource_group" "rg1" {
  name     = "Firstresource"
  location = "West Europe"

  tags = {
    environment = "production"
  }
}

resource "azurerm_app_service_plan" "app_plan" {
  name                = "FirstAppserviceplan"
  location            = azurerm_resource_group.rg1.location
  resource_group_name = azurerm_resource_group.rg1.name

  sku {
    tier = "Standard"
    size = "S1"
  }
}

resource "azurerm_app_service" "webapp" {
  name                = "temenosappservice"
  location            = azurerm_resource_group.rg1.location
  resource_group_name = azurerm_resource_group.rg1.name
  app_service_plan_id = azurerm_app_service_plan.app_plan.id

  site_config {
    dotnet_framework_version = "v4.0"
    scm_type                 = "LocalGit"
  }

  app_settings = {
    "SOME_KEY" = "some-value"
  }

  connection_string {
    name  = "Database"
    type  = "SQLAzure"
    value = "Server=tcp:azurerm_sql_server.sqlserver.fully_qualified_domain_name Database=azurerm_sql_database.db.name;User ID=azurerm_sql_server.sqlserver.administrator_login;Password=azurerm_sql_server.sqlserver.administrator_login_password;Trusted_Connection=False;Encrypt=True;"
  }
}

resource "azurerm_sql_server" "sqlserver" {
  name                         = "temenossqlserver"
  resource_group_name          = azurerm_resource_group.rg1.name
  location                     = azurerm_resource_group.rg1.location
  version                      = "12.0"
  administrator_login          = var.sql_admin_login
  administrator_login_password = var.sql_admin_password
}

resource "azurerm_sql_database" "db" {
  name                = "Firstsqldb"
  resource_group_name = azurerm_resource_group.rg1.name
  location            = azurerm_resource_group.rg1.location
  server_name         = azurerm_sql_server.sqlserver.name
}
