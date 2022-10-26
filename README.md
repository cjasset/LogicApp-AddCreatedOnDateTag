# Azure Logic App to Create CreatedOnDate and ModifiedOnDate tags
## Summary
This solution creates a logic app that lists all resources in a subscription from the Azure Resource Manager API (https://learn.microsoft.com/en-us/rest/api/resources/resources/list).  It then loops through each resource and adds two tags, CreatedOnDate and ModifiedOnDate using the createdTime and changedTime properties retrieved from the API. 

## Background
Many organizations struggle with tracking resource sprawl in Azure.  In many cases the first step in tracking resources is tracking when they were created and when they were last modified.  Azure does not have an easy way to view this data.  It is not viewable on the resource in the portal, nor is it viewable in the Azure Resource Graph explorer.  The only way to view this data is using the REST API and explicitly requesting the createdTime and changedTime properties.  There are many solutions that make this data more consumable via adding the metadata to the resources as Tags.  The best way to do this is via Azure Policy such as the example in this blog post (https://jrudlin.github.io/2019-07-18-azure-policy-createdon-date/).  Unfortunately the Azure Policy solution only works for new resources created, and in the case of pre-existing resources, the policy will update the tag with the modified data instead of the creation date which leads to data quality issues.  This solution serves to bridge the gap and provide an accurate means to retroactively tag all pre-existing resources with the appropriate creationTime.  Once all resources are tagged, Azure Policy should be used to maintain tag consistency going forward.

## Resources Created
* Logic App
* API Connection
* System Assigned Managed Identity (For the Logic App)
* Azure RBAC assignment granting Tag Contributor to the Logic App Managed Identity

## Known issues
* Will error on resources that do not support tags
    * There is a filter in the Logic App ARM Connector to filter out resources that do not support tags to prevent these errors.  If you are getting errors on certain resources, check if the resource supports tags and if not, add to the filter.

### Installation with Azure Portal

Click the Deploy To Azure button below.

[![Deploy to Azure](https://aka.ms/deploytoazurebutton)](https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2cjasset%2LogicApp-AddCreatedOnDateTag%2main%2LogicApp-AddCreatedOnDateTag.json)