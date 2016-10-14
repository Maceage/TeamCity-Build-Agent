# TeamCity EC2 Build Agent Setup

This repository contains all the resources, packages and information needed to create an AWS EC2 TeamCity Build Agent without installing Visual Studio

Additional help available on the following StackOverflow thread:
[Running Code Analysis (FxCop 12.0 / 14.0) on build agent without installing Visual Studio 2013 / 2015](http://stackoverflow.com/questions/21729066/running-code-analysis-fxcop-12-0-14-0-on-build-agent-without-installing-visu/32093939#32093939)

Official Jetbrains TeamCity documentation can be found here:
[Setting Up TeamCity for Amazon EC2](https://confluence.jetbrains.com/display/TCD9/Setting+Up+TeamCity+for+Amazon+EC2#SettingUpTeamCityforAmazonEC2-PreparingImagewithInstalledTeamCityAgent)

## Install applications:

* [Google Chrome](https://www.google.com.au/chrome/browser/desktop/)
* [CCleaner](https://www.piriform.com/ccleaner/download)
* [Notepad++](https://notepad-plus-plus.org/)
* [.NET Build Tools](https://www.microsoft.com/en-us/download/details.aspx?id=48159)
* [.NET Developer Packs](http://getdotnet.azurewebsites.net/target-dotnet-platforms.html)
* Visual Studio MSBuild Templates (In this Git repository)
* [Git](https://git-scm.com/download/win) (for client-side checkout
* [Visual C++ Redistributable for Visual Studio 2015 x86](http://www.microsoft.com/en-us/download/confirmation.aspx?id=48145)
* [MSBuild Community Tasks](https://github.com/loresoft/msbuildtasks)
* [Windows 10 SDK](https://dev.windows.com/en-us/downloads/windows-10-sdk)
* Install TeamCity Build Agent (From TeamCity Web UI)

## Configure Windows Firewall rule

Open inbound port 9090 on Windows Firewall for TeamCity Build Agent

## Extract MSBuild Templates

Extract VisualStudio.zip file to the following path:

`C:\Program Files (x86)\MSBuild\Microsoft`

## Install FxCop

* Run the FxCop installer (WinSDK_FxCopSetup.exe) from this repository
* Using gacutil.exe register the following DLLs:
  * Microsoft.VisualStudio.CodeAnalysis.dll
  * Microsoft.VisualStudio.CodeAnalysis.Sdk.dll

Command: `gacutil -i [path-to-dll]`
Gacutil Location: `C:\Program Files\Microsoft SDKs\Windows\v[x]\bin`

## Import certificate

[Download Lets Encrypt Root Certificate](https://letsencrypt.org/certificates/) in .pem format

### TeamCity

Import certificate into TeamCity Agent JDK certificate store:
`keytool -importcert -file <cert file> -keystore <path to JRE installation>/lib/security/cacerts`
(Password: changeit)

### Windows

* Start -> Run
* certmgr.msc
* Trusted Root Certification Authorities -> Certificates
* Right-click -> All Tasks -> Import
* Find and import downloaded certificate

## EC2-specific Commands

`sc config TCBuildAgent depend= EC2Config`

## Prepare image

* Ensure TeamCity Build Agent can communicate with TeamCity Server
* Clean image with CCleaner
* Open `C:\BuildAgent`
* Copy `buildAgent.properties` file to make a backup
* Start -> Run -> `services.msc`
* Stop the TeamCity Build Agent
* Delete `C:\BuildAgent\logs` directory
* Delete `C:\BuildAgent\temp` directory
* Edit `buildAgent.properties` file
* Remove `name, serverUrl, authorizationToken` properties
* Stop the TeamCity Build Agent and create an AMI
* Add the AMI to TeamCity Agent Cloud configuration
