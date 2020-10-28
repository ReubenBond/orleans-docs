---
layout: page
title: Multi-Cluster Silo Configuration
---

## Orleans Silo Configuration

To get a quick overview, we show all relevant configuration parameters:

```csharp
var silo = new SiloHostBuilder()
  [...]
  .Configure<ClusterInfo>(options =>
  {
    options.ClusterId = "us3";
    options.ServiceId = "myawesomeservice";
  })
  .Configure<MultiClusterOptions>(options => 
  {
    options.HasMultiClusterNetwork = true;
    options.DefaultMultiCluster = new[] { "us1", "eu1", "us2" };
    options.BackgroundGossipInterval = TimeSpan.FromSeconds(30);
    options.UseGlobalSingleInstanceByDefault = false;
    options.GlobalSingleInstanceRetryInterval = TimeSpan.FromSeconds(30);
    options.GlobalSingleInstanceNumberRetries = 3;
    options.MaxMultiClusterGateways = 10;
    options.GossipChannels.Add("AzureTable", "DefaultEndpointsProtocol=https;AccountName=usa;AccountKey=...");
    options.GossipChannels.Add("AzureTable", "DefaultEndpointsProtocol=https;AccountName=europe;AccountKey=...")
    [...]
  })
  [...]
```

The `Service Id` is an arbitrary ID for identifying this service. It must be the same for all clusters and all silos. 

The `MultiClusterNetwork` section is optional - if not present, all multi-cluster support is disabled for this silo.

The **required parameters** `ClusterId` and `GossipChannel` are explained in the section on [Multi-Cluster Communication](GossipChannels.md).

The  optional parameters `MaxMultiClusterGateways` and `BackgroundGossipInterval`  are explained in the section on [Multi-Cluster Communication](GossipChannels.md).

The optional parameter `DefaultMultiCluster` is explained in the section on [Multi-Cluster Configuration](MultiClusterConfiguration.md).

The optional parameters `UseGlobalSingleInstanceByDefault`,  `GlobalSingleInstanceRetryInterval` and `GlobalSingleInstanceNumberRetries` are explained in the section on [Global-Single-Instance Grains](GlobalSingleInstance.md).


## Orleans Client Configuration

No extra configuration is required for Orleans client. The same client may not connect to silos in different clusters (the silo refuses the connection in that situation). 