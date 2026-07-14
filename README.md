Deploy an Azure VM Scale set with load balancing using Terraform.

Deploy an Azure VM Scale set with load balancing using Terraform.




Introduction

1. Use an existing subnet as a data source
2. Create and configure an Azure load balancer
3. Create and configure an Azure VM Scale Set
By the end of this lab, you will have created an Azure VM Scale Set and load balancer using Terraform and a module from the public registry.

In this lab we are going to create the Azure load balancer using the terraform module and configure the Vmss in the backend pool of the load balancer.

Vmss is configured with  2 instance (Manual scaling ) 

How a website in both of the instance and access the website using the frontend IP of the load balancer through internet.

Hosting the website in the Linux machine.
 
With the help of the Custom date we execute or script that hold the HTML file.

custom_data is an argument you set on an Azure VM or VMSS resource — it's Azure's mechanism for handing a script or configuration to a virtual machine so it runs automatically the first time that VM boots. This is exactly what we walked through earlier in your project:

hcl
custom_data = base64encode(templatefile("${path.module}/custom_data.tpl", {
  admin_username = var.admin_username
  port           = var.application_port
}))

I have crate the file name call Custom_data.tpl in the Visual code their I put my script that run while creating the VMSS.




Step 1 :   lab diagram 



First we connect the terraform with azure environment .

Here I am using the azure Cli to connect.

    Ø  az login 

Step 2 : As my resource group and virtual network is already created through the azure poral.

So I have to informed my terraform about my created resource.

Using the data bock for read the existing resource.

A data block in Terraform is how you read/query information about a resource that already exists, rather than create a new one. This is exactly what your main.tf uses:


hcl
data "azurerm_resource_group" "vmss" {
  name = var.resource_group_name
}
data "azurerm_subnet" "web" {
  resource_group_name  = data.azurerm_resource_group.vmss.name
  virtual_network_name = var.vnet_name
  name                 = var.subnet_name
}

Terraform.tfvars file help here .

terraform.tfvars is a file that supplies actual values for the input variables you declared in variables.tf. It's the piece that separates "what values my config accepts" (declaration) from "what values I'm actually using right now" (assignment).
The relationship to variables.tf
Your variables.tf declares variables — their names, types, descriptions, and optional defaults — but doesn't necessarily give them real values:

hcl
variable "resource_group_name" {
  description = "Name of resource group provided by the lab."
  type        = string
}
Notice this one has no default — meaning Terraform has no idea what value to use unless you supply one somewhere. That's exactly what terraform.tfvars does:

hcl
resource_group_name = "943-aa222c50-deploy-an-azure-vm-scale-set-with-loa"
vnet_name           = "some-vnet-name"

Terraform matches these by variable name and plugs the value in wherever var.resource_group_name or var.vnet_name is referenced throughout your .tf files (like in data.azurerm_resource_group.vmss).


Step 3 : Creating the load balancer using the module.





Step 4 : Creating the VMSS







We are able to access the  load balancer  frontend IP address.




