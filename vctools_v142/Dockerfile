# escape=`

# # Use the latest Windows Server Core image with .NET Framework 4.8.
# FROM mcr.microsoft.com/dotnet/framework/sdk:4.8-windowsservercore-ltsc2019
FROM mcr.microsoft.com/windows/servercore:ltsc2019

SHELL ["powershell", "-Command", "$ErrorActionPreference = 'Stop'; $ProgressPreference = 'SilentlyContinue';"]

ENV VISUAL_STUDIO_VERSION=16

RUN $url = ('https://aka.ms/vs/{0}/release/vs_BuildTools.exe' -f $env:VISUAL_STUDIO_VERSION); `
    Write-Host ('Downloading {0} ...' -f $url); `
    Invoke-WebRequest -Uri $url -OutFile vs_BuildTools.exe; `
    Write-Host ('Installing Visual Studio {0} ...' -f $env:VISUAL_STUDIO_VERSION); `
    Start-Process -FilePath 'vs_BuildTools.exe' -Wait -ArgumentList @( `
    '--installPath C:/MSVC/', `
    '--includeRecommended', `
    '--add Microsoft.VisualStudio.Workload.VCTools', `
    '--quiet', '--wait', '--norestart', '--nocache' `
    ); `
    Write-Host 'Removing temporary files ...'; `
    Remove-Item -Force 'vs_BuildTools.exe' ; `
    $buildToolsVersion = & 'C:\Program Files (x86)\Microsoft Visual Studio\Installer\vswhere.exe' -property catalog_productDisplayVersion -path C:\MSVC ; `
    Write-Host "Installed version:$buildToolsVersion" ; `
    Remove-Item -Force -Recurse 'C:\Program Files (x86)\Microsoft Visual Studio\Installer'; `
    Write-Host 'Complete.';


ENTRYPOINT ["C:\\MSVC\\Common7\\Tools\\VsDevCmd.bat", "&&", "powershell.exe", "-NoLogo", "-ExecutionPolicy", "Bypass"]
