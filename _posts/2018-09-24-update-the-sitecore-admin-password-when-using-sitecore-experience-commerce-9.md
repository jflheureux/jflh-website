---
title: Update the Sitecore Admin Password When Using Sitecore Experience Commerce 9
tags: [Sitecore, Sitecore Experience Commerce]
sharing:
  linkedin: Sitecore Experience Commerce stops working after updating the Sitecore admin password. See how to fix the issue.
---

Updating a Sitecore user password is very simple when using only the Sitecore Experience Platform. However, there are undocumented additional steps to execute when using Sitecore Experience Commerce 9.

<!-- more -->

## Problem

Sitecore Experience Commerce 9 keeps the Sitecore admin username and password in the SQL database and uses them frequently. When changing the Sitecore admin password, the account gets locked quickly and the Commerce features are returning errors in Sitecore. This exception can be found in the Sitecore logs:

```
10548 2018:09:24 16:51:55 ERROR An error occurred while processing this request.
Exception: Microsoft.OData.Client.DataServiceQueryException
Message: An error occurred while processing this request.
Source: Sitecore.Commerce.ServiceProxy
   at Sitecore.Commerce.ServiceProxy.Proxy.GetValue[T](DataServiceQuerySingle`1 query)
   at Sitecore.Commerce.Engine.Connect.Search.IndexUtility.<>c__DisplayClass12_0.<GetEnvironmentArtifactStoreId>b__0()
   at Sitecore.Commerce.Engine.Connect.Search.IndexUtility.ExecuteWithRetry(Action action)
   at Sitecore.Commerce.Engine.Connect.Search.IndexUtility.GetEnvironmentArtifactStoreId(String environmentName)
   at Sitecore.Commerce.Engine.Connect.Search.Strategies.CatalogSystemIntervalAsynchronousStrategyBase`1.IndexDeletedItems()
   at Sitecore.Commerce.Engine.Connect.Search.Strategies.CommerceIntervalAsynchronousStrategy.Run()
   at Sitecore.ContentSearch.Maintenance.OperationMonitor.ExecuteActions(ConcurrentQueue`1 actions, String groupName)

Nested Exception

Exception: Microsoft.OData.Client.DataServiceClientException
Message: {
  "@odata.context":"https://localhost:5000/Api/$metadata#Sitecore.Commerce.Core.CommandMessage","MessageDate":"2018-09-24T16:51:55.9536942Z","Code":"Error","Text":"Shop 'CommerceEngineDefaultStorefront' does not exist.","CommerceTermKey":"InvalidShop"
}
Source: Microsoft.OData.Client
   at Microsoft.OData.Client.BaseAsyncResult.EndExecute[T](Object source, String method, IAsyncResult asyncResult)
   at Microsoft.OData.Client.QueryResult.EndExecuteQuery[TElement](Object source, String method, IAsyncResult asyncResult)
```

## Solution

TL;DR: The password must be changed in the 4 Sitecore Commerce services bootstraping configuration and bootstrapping must be done again.

Sitecore Experience Commerce use the admin password in 4 of its services:
* CommerceAuthoring
* CommerceMinions
* CommerceOps
* CommerceShops

These services are by default installed in `C:\inetpub\wwwroot\`.

1. For each of these 4 Sitecore Commerce services:
    1. Open the bootstraping configuration file: `\wwwroot\bootstrap\Global.json`.
    2. In the `Policies > $values` array, find the object with the `$type` property value of `Sitecore.Commerce.Plugin.Management.SitecoreConnectionPolicy, Sitecore.Commerce.Plugin.Management`. It is typically the last one.
    3. Change the `Password` property value.
    4. Save the file.
2. Bootstrap the Commerce services again. This can be done using the Postman collection from the Sitecore Experience Commerce SDK. Follow the steps from Naveed Ahmad blog: [https://naveed-ahmad.com/2018/02/21/sitecore-experience-commerce-xc9-getting-started-with-postman/](https://naveed-ahmad.com/2018/02/21/sitecore-experience-commerce-xc9-getting-started-with-postman/)

> **Note:** You might have to unlock the Sitecore admin account if Sitecore Experience Commerce tried to use its old password too often.