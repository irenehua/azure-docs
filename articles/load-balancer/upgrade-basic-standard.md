---
title: Upgrade from Basic Public to Standard Public - Azure Load Balancer
description: This article shows you how to upgrade Azure Public Load Balancer from Basic SKU to Standard SKU
services: load-balancer
author: irenehua
ms.service: load-balancer
ms.topic: article
ms.date: 01/23/2020
ms.author: irenehua
---

# Upgrade Azure Public Load Balancer from Basic SKU to Standard SKU
[Azure Standard Load Balancer](load-balancer-overview.md) is now available, offering additional features such as bigger scale and highly available services. However, existing Basic aren't automatically upgraded to Standard. If you want to migrate from Basic Public Load Balancer to Standard Public Load Balancer, follow the steps in this article.

There are two stages in a upgrade:

1. Migrate the configuration
2. Add VMs to backend pools of Standard Load Balancer

This article covers configuration migration. Adding VMs to backend pools of Standard Load Balancer varies depending on your specific environment. However, some high-level, general recommendations [are provided](#migrate-client-traffic).

## Upgrade overview

An Azure PowerShell script is available that does the following:

* Creates a Standard SKU Public Load Balancer in the resource group and location the you specify. 
* Seamlessly copies the configurations of the Basic SKU Public Load Balancer to the newly create Standard Public Load Balancer.
* Creates a default outbound rule for the newly created Standard Public Load Balancer. 

### Caveats\Limitations

* Script only supports Public Load Balancer upgrade. For Internal Basic Load Balancer upgrade, please create a Standard Internal Load Balancer if outbound connectivity is not desired, and please create a Standard Internal Load Balancer and Standard Public Load Balancer if outbound connectivity is required.
* The new Standard Load Balancer has a new public and private IP address. It’s impossible to move the IP addresses associated with existing Basic Load Balancer seamlessly to Standard Load Balancer since they are different SKUs.
* If the Standard load balancer is created in a different region, you won’t be able to associate the VMs existing in the old region to the newly created Standard Load Balancer. To work around this limitation, make sure to create a new VM in the new region.
* If you have FIPS mode enabled for your V1 gateway, it won’t be migrated to your new v2 gateway. FIPS mode isn't supported in v2.
* If your LB does not have any Frontend IP configuration or backend pool, you are likely to hit an error running the script. Please make sure they are not empty.

## Download the script

Download the migration script from the  [PowerShell Gallery](https://www.powershellgallery.com/packages/AzureAppGWMigration).

