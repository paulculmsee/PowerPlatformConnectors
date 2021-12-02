## Power Automate Flow Runs Connector
Power Automate provides a Management Connector, but this does not provide a means to get details on the Runs made by flows. This connecter rectifies this gap by providing the ability to enumerate runs for a specified flow.

## Prerequisites
You will need the following to proceed:
* A Microsoft Power Apps or Power Automate plan with premium features emabled
* An Azure subscription
* The Power platform CLI tools

## Building the connector 
Since Flow Service API's are secured by Azure Active Directory (AD), we first need to set up a few thing in Azure AD so that our connectors can securely access it.  After that is completed, you can create and test the connector.

### Set up an Azure AD application for your custom connector
We first need to register our connector as an application in Azure AD.  This will allow the connector to identify itself to Azure AD so that it can ask for permissions to access Flow Management data on behalf of the end user.  You can read more about this [here](https://docs.microsoft.com/en-us/azure/active-directory/develop/authentication-scenarios) and follow the steps below:

1. Create an Azure AD application
This Azure AD application will be used to identify the connector to Azure Key Vault.  This can be done using [Azure Portal] (https://portal.azure.com), by following the steps [here](https://docs.microsoft.com/en-us/azure/active-directory/develop/quickstart-register-app).  Once created, note down the value of Application (Client) ID.  You will need this later.

2. Configure (Update) your Azure AD application to access the Flow Management API
This step will ensure that your application can successfully retrieve an access token to invoke the Get Flow Runs API on behalf of your users.  To do this, follow the steps [here](https://docs.microsoft.com/en-us/azure/active-directory/develop/quickstart-configure-app-access-web-apis).
    - For redirect URI, use “https://global.consent.azure-apim.net/redirect”
    - For the credentials, use a client secret (and not certificates).  Remember to note the secret down, you will need this later and it is shown only once.
    - For API permissions, make sure “Flow Service” and “Activity.Read.All" and "Flows.Read.All” are added.
   
At this point, we now have a valid Azure AD application that can be used to get permissions from end users and access Flow Service.  The next step for us is to create a custom connector.

### Deploying the sample
Run the following commands and follow the prompts:

```paconn
paconn create --api-def apiDefinition.swagger.json --api-prop apiProperties.json --secret <client_secret>
```

## Supported Operations
The connector supports the following operations:
* `Get Flow Runs`: List keys in the specified vault




