terraform {
  required_providers {
    azurerm = {
      source  = "hashicorp/azurerm"
      version = "=3.51.0"
    }
  }
}

# provider 설정
provider "azurerm" {
  features {}

client_id         =""
subscription_id   =""
tenant_id         =""
client_secret     =""

}

# 리소스 그룹 생성
resource "azurerm_resource_group" "myrg" {
  name     = "projectRG"
  location = "East US"
}

# 가상 네트워크 생성
resource "azurerm_virtual_network" "myvnet" {
  name                = "projectVN"
  address_space       = ["10.0.0.0/16"]
  location            = "${azurerm_resource_group.myrg.location}"
  resource_group_name = "${azurerm_resource_group.myrg.name}"
}

# 서브넷 생성
resource "azurerm_subnet" "mysubnet" {
  name                 = "AKS"
  resource_group_name  = "${azurerm_resource_group.myrg.name}"
  virtual_network_name = "${azurerm_virtual_network.myvnet.name}"
  address_prefixes       = ["10.0.1.0/24"]
  
}

# AKS 클러스터 생성
resource "azurerm_kubernetes_cluster" "aks" {
  name                = "myaks"
  location            = "${azurerm_resource_group.myrg.location}"
  resource_group_name = "${azurerm_resource_group.myrg.name}"
  dns_prefix          = "myaks"

  default_node_pool {
    name            = "default"
    node_count      = 2
    vm_size         = "Standard_DS2_v2"
    os_disk_size_gb = 30
  }

  service_principal {
    client_id     = ""
    client_secret = ""
  }

  tags = {
    Environment = "Dev"
  }
}
