# Autopilot-Manager

Autopilot-Manager needs the [Autopilot-Manager-Client](https://github.com/okieselbach/Autopilot-Manager-Client) to receive the Autopilot device provisioning information import request and showing the user a processing screen similar to the Autopilot Pre-Provisioning scenario (former known as WhiteGlove). The app service handles all the processing to import the device provisioning information into the tenant. It uses the same logic like the Michael Niehaus Autopilot script [Get-WindowsAutoPilotInfo](https://www.powershellgallery.com/packages/Get-WindowsAutoPilotInfo). The process is described in a blog post from Michael here: [Importing a device hash directly into Intune](https://oofhours.com/2020/03/25/importing-a-device-hash-directly-into-intune/)

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

> **Note:** after deployment the app service needs to be stopped and started (not using the restart button) in the Azure portal. Otherwise the ASP.NET Core Extension is not recognized and a depenency error will occur.

<br />

<a href="https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2Fokieselbach%2FAutopilot-Manager%2Fmaster%2Fazuredeploy.json" target="_blank">
    <img src="https://aka.ms/deploytoazurebutton"/>
</a>

