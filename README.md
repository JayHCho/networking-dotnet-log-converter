---
services: virtual-network
platforms: dotnet
author: JayHCho
---

# Azure Networking Log Converter

Code for downloading operational network logs and converting them to .CSV files.  Files can to be then uploaded to Power BI for analysis.
## Running this sample
### Prerequisites:

* Visual Studio 2015
* .NET Framework 4.6
* Microsoft Azure SDK - [Latest](https://azure.microsoft.com/en-us/downloads/)
* Cloud Explorer for Visual Studio 2015 - [Visual Studio Extension](https://visualstudiogallery.msdn.microsoft.com/84e83a7c-9606-4f9f-83dd-0f6182f13add) (recommended)
 
## Solution Contents
The solution contains 3  executable projects CountersLogConverter, EventsLogConverter and OperationsLogConverter.


#####1.  CountersLogConverter

In order to use this code, logging must be turned on via SDK or Ibiza portal (soon to be released)
and familiarity with Azure Resource Manager is required.

The counters logs are stored in the Azure Storage Container as JSON blobs
```
{
   "time": "2015-09-11T23:14:22.6940000Z",
   "systemId": "e22a0996-e5a7-4952-8e28-4357a6e8f0c5",
   "category": "NetworkSecurityGroupRuleCounter",
   "resourceId": "/SUBSCRIPTIONS/D763EE4A-9131-455F-8C5E-876035455EC4/RESOURCEGROUPS/INSIGHTOBONRPFOO/PROVIDERS/MICROSOFT.NETWORK/NETWORKSECURITYGROUPS/NSGINSIGHTOBONRPFOO",
   "operationName": "NetworkSecurityGroupCounters",
   "properties": {
                  "vnetResourceGuid":"{DD0074B1-4CB3-49FA-BF10-8719DFBA3568}",
                  "subnetPrefix":"10.0.0.0/24",
                  "macAddress":"001517D9C43C",
                  "ruleName":"DenyAllOutBound",
                  "direction":"Out",
                  "type":"block",
                  "matchedConnections":0
                 }
}
```

Converted .CSV file of counter log has following columns

1. *time*
2. *systemId*
3. *resourceId*
4. *operationName*
5. *properties.vnetResourceGuid*
6. *properties.subnetPrefix*
7. *properties.macAddress*
8. *properties.ruleName*
9. *properties.direction*
10. *properties.type*
11. *properties.matchedConnections*

---

#####2.  EventsLogConverter

In order to use this code, logging must be turned on via SDK or Ibiza portal (soon to be released)
and familiarity with Azure Resource Manager is required.

The events logs are stored in the Azure Storage Container as JSON blobs
```
{
   "time": "2015-09-11T23:05:22.6860000Z",
   "systemId": "e22a0996-e5a7-4952-8e28-4357a6e8f0c5",
   "category": "NetworkSecurityGroupEvent",
   "resourceId": "/SUBSCRIPTIONS/D763EE4A-9131-455F-8C5E-876035455EC4/RESOURCEGROUPS/INSIGHTOBONRPFOO/PROVIDERS/MICROSOFT.NETWORK/NETWORKSECURITYGROUPS/NSGINSIGHTOBONRPFOO",
   "operationName": "NetworkSecurityGroupEvents",
   "properties": {
                  "vnetResourceGuid":"{DD0074B1-4CB3-49FA-BF10-8719DFBA3568}",
                  "subnetPrefix":"10.0.0.0/24",
                  "macAddress":"001517D9C43C",
                  "ruleName":"AllowVnetOutBound",
                  "direction":"Out",
                  "priority":65000,
                  "type":"allow",
                  "conditions":{
                                "destinationPortRange":"0-65535",
                                "sourcePortRange":"0-65535",
                                "destinationIP":"10.0.0.0/8,172.16.0.0/12,169.254.0.0/16,192.168.0.0/16,168.63.129.16/32",
                                "sourceIP":"10.0.0.0/8,172.16.0.0/12,169.254.0.0/16,192.168.0.0/16,168.63.129.16/32"
                                }
                 }
}
```

Converted .CSV file of events log has following columns

1. *time*
2. *systemId*
3. *resourceId*
4. *operationName*
5. *properties.vnetResourceGuid*
6. *properties.subnetPrefix*
7. *properties.macAddress*
8. *properties.ruleName*
9. *properties.direction*
10. *properties.priority*
11. *properties.type*
12. *properties.conditions.destinationPortRange*
13. *properties.conditions.sourcePortRange*
14. *properties.conditions.sourceIP*
15. *properties.conditions.destinationIP*
16. *properties.conditions.protocols*

---

#####3.  OperationsLogConverter

This app needs to be authorized to access Azure AD management API.
Fore more information on Azure AD authorization go to: https://msdn.microsoft.com/en-us/library/azure/dn790557.aspx

Operations logs are retrieved through the Azure Insight API.  For available properties look at member of [Microsoft.Azure.Insight.Models.EventData class definition](https://msdn.microsoft.com/en-us/library/azure/microsoft.azure.insights.models.eventdata.aspx).

Converted .CSV file of operations log has following columns.

1. *SubscriptionId*
2. *EventTimeStamp*
3. *EventDate*
4. *EventDataId*
5. *CorrelationId*
6. *EventName*
7. *Level*
8. *ResourceGroupName*
9. *ResourceProviderName*
10. *ResourceId*
11. *ResourceName*
12. *ResourceLocation*
13. *Status*
14. *Caller*
15. *OperationId*
16. *OperationName*
17. *OperationRP*
18. *OperationResType*
19. *OperationType*
20. *Description*
21. *Title*
22. *Service*
23. *Region*
24. *Transcript*
25. *IncidentId*
26. *IncidentType*

---

