# Autopilot Manager (APM)

Autopilot Manager (APM) needs the [Autopilot-Manager-Client](https://github.com/okieselbach/Autopilot-Manager-Client) to receive the Autopilot device provisioning information import request and showing the user a processing screen, similar to the Autopilot Pre-Provisioning scenario (former known as WhiteGlove). The app service queues and handles all the processing to import the device provisioning information into the tenant. It has an approval workflow built in via QR code or Approval helpdesk page. It uses the same logic like the Michael Niehaus Autopilot script [Get-WindowsAutoPilotInfo](https://www.powershellgallery.com/packages/Get-WindowsAutoPilotInfo). The process of the Get-WindowsAutoPilotInfo script is described in a blog post from Michael here: [Importing a device hash directly into Intune](https://oofhours.com/2020/03/25/importing-a-device-hash-directly-into-intune/)

Read more about the solution and detailed installation instructions on my blog post here: **[Introducing Autopilot Manager](https://oliverkieselbach.com/2020/12/08/autopilot-manager/)**

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
