# TeamCity EC2 Build Agent Setup

Official Jetbrains TeamCity documentation can be found here:
[Setting Up TeamCity for Amazon EC2](https://confluence.jetbrains.com/display/TCD9/Setting+Up+TeamCity+for+Amazon+EC2#SettingUpTeamCityforAmazonEC2-PreparingImagewithInstalledTeamCityAgent)

## Install applications:

* [Google Chrome](https://www.google.com.au/chrome/browser/desktop/)
* [CCleaner](https://www.piriform.com/ccleaner/download)
* [Notepad++](https://notepad-plus-plus.org/)
* [.NET Build Tools](https://www.microsoft.com/en-us/download/details.aspx?id=48159)
* [.NET Developer Packs](http://getdotnet.azurewebsites.net/target-dotnet-platforms.html)
* FxCop (In Git this repository)
* Visual Studio MSBuild Templates (In this Git repository)
* [Git](https://git-scm.com/download/win) (for client-side checkout
* Install TeamCity Build Agent (From TeamCity Web UI)

## Configure Windows Firewall rule

Open inbound port 9090 on Windows Firewall for TeamCity Build Agent

## Extract MSBuild Templates

Extract VisualStudio.zip file to the following path:

`C:\Program Files (x86)\MSBuild\Microsoft`

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
