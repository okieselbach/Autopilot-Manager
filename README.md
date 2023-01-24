# Autopilot Manager (APM)

Autopilot Manager (APM) needs the [Autopilot-Manager-Client](https://github.com/okieselbach/Autopilot-Manager-Client) to receive the Autopilot device provisioning information import request and showing the user a processing screen, similar to the Autopilot Pre-Provisioning scenario (former known as WhiteGlove). The app service queues and handles all the processing to import the device provisioning information into the tenant. It has an approval workflow built in via QR code or Approval helpdesk page. It uses the same logic like the Michael Niehaus Autopilot script [Get-WindowsAutoPilotInfo](https://www.powershellgallery.com/packages/Get-WindowsAutoPilotInfo). The process of the Get-WindowsAutoPilotInfo script is described in a blog post from Michael here: [Importing a device hash directly into Intune](https://oofhours.com/2020/03/25/importing-a-device-hash-directly-into-intune/)

Read more about the solution and detailed installation instructions on my blog post here:

* **[Introducing Autopilot Manager](https://oliverkieselbach.com/2020/12/08/autopilot-manager/)**
* **[Evolving Autopilot Manager](https://oliverkieselbach.com/2021/12/21/evolving-autopilot-manager/)**

<img src="https://oliverkieselbach.files.wordpress.com/2020/12/autopilotmanagerandclient-1-e1607072950726.png?w=1100"/>

## Prerequisites

The following prerequisites are necessary to get Autopilot-Manager to work:

* Azure AD Application Registration Client-ID
* Azure AD Application Registration Client-Secret
* Azure AD group for Autopilot direct profile assignment
* Azure AD group for general Autopilot-Manager access
* Azure AD group for 'View-Imports' access (Job Histroy Viewer)
* Azure AD group for 'Approve-Requests' access (Approver)

## Deployment
The app service can be deployed via the Azure Resource Manager (ARM) template by using the following link:

<br />

<a href="https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2Fokieselbach%2FAutopilot-Manager%2Fmaster%2Fazuredeploy.json" target="_blank">
    <img src="https://aka.ms/deploytoazurebutton"/>
</a>

## Changelog
v1.6
- added support for Azure Functions, which gives extended functionality developed at business side.</br>
  _AppConfig:AutopilotManagerConfig:AzureFunctionsUrl_ set to your Azure Function URL (e.g. https://apm-functions-xxx.azurewebsites.net/api/)</br>
  - A validation function (function name: '**validate**') to be called for extended validation of the import request. Validation result will allow or block import</br>
  _AppConfig:AutopilotManagerConfig:UseValidationFunction_ set to true</br>
  _AppConfig:AutopilotManagerConfig:ValidationFunctionKey_ set to "your azure function key"</br>
  - A GroupTag function (function name: '**grouptag**') to receive a GroupTag for the given device</br>
  _AppConfig:AutopilotManagerConfig:UseGroupTagFunction_ set to true</br>
  _AppConfig:AutopilotManagerConfig:GroupTagFunctionKey_ set to "your azure function key"</br>
  - An AutoApproval function (function name: '**autoapproval**') to automatically approve requests after extended validation</br>
  _AppConfig:AutopilotManagerConfig:UseAutoApprovalFunction_ set to true</br>
  _AppConfig:AutopilotManagerConfig:AutoApprovalFunctionKey_ set to "your azure function key"</br>

v1.5
- added support for re-register of Autopilot devices. Existing Autopilot devices will be deleted upfront before upload of new Autopilot device information.</br>
  Tun on ReRegister mode with</br>
  _AppConfig:AutopilotManagerConfig:UseReRegisterMode_ set to true</br>
  Additional App registration permissions "_DeviceManagementManagedDevices.ReadWrite.All_" must be granted, same as for delete requests</br>
  To preservce the purchase order identifier in case of re-register use</br>
  _AppConfig:AutopilotManagerConfig:PreservePurchaseOrderIdOnReRegister_ set to true</br>
- added support for writing Audit data to Log Analytics via data collector API</br>
  use the following configurations to configure this</br>
  _AppConfig:AutopilotManagerConfig:UseLogAnalytics_ set to true</br>
  _AppConfig:AutopilotManagerConfig:LogAnalyticsWorkspaceId_ set to "your workspace GUID"</br>
  _AppConfig:AutopilotManagerConfig:LogAnalyticsSharedKey_ set to "your shared workspace key"</br>
  _AppConfig:AutopilotManagerConfig:LogAnalyticsReportHardwareHash_ set to true or false</br>
- added Homepage customization options</br>
  _AppConfig:AutopilotManagerConfig:HomepageHeadlineSentenceApprovalMode_ to e.g. "Please call the helpdesk (+49 180-12345678) for approval of device import."</br>
  _AppConfig:AutopilotManagerConfig:HomepageHeadlineSentenceNonApprovalMode_ to e.g. "Please scan the QR code to import the device."</br>
  _AppConfig:AutopilotManagerConfig:HomepageCompanyLogoImageUrl_ to an image url "https://company.com/image/comapnylogo.png"</br>


v1.4
- added deletion support in Approval Mode for Intune devices due to this latest change: </br>
  https://docs.microsoft.com/en-us/mem/autopilot/troubleshoot-device-enrollment</br>
  new client parameter **-e** can be used to invoke the delete request</br>
  _AppConfig:AutopilotManagerConfig:AllowDeletionInApprovalMode_ must be set to true</br>
  Additional App registration permissions "_DeviceManagementManagedDevices.ReadWrite.All_" must be granted</br>
- Optionally you can display the Approval and History link now on the footer area of the main page</br>
  _AppConfig:AutopilotManagerConfig:ShowHomepageApprovalLink_ set to true</br>
  _AppConfig:AutopilotManagerConfig:ShowHomepageHistoryLink_ set to true</br>


v1.3
- added timeout configuration option</br>
  _AppConfig:AutopilotManagerConfig:Timeout_ must be set to integer in minute e.g. 120 minutes


v1.2
- improved model/manufacturer parsing
- improved AAD group membership addition
