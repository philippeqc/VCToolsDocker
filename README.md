
VCTools build environment in Docker

Provide the workload Microsoft.VisualStudio.Workload.VCTools, currently v142 using Visual Studio 2019 build tools on Windows Server 2019.

# Usage

```powershell
docker pull philippeqc/vctools:latest
```

# Build and publish

```powershell
$baseImage="ltsc2019"
$vcEdition="v142"
$latestTag="$vcEdition-$baseImage-latest"
docker pull mcr.microsoft.com/windows/servercore:$baseImage

docker build -t vctools:$latestTag -m 6GB ./vctools$vcEdition --network "Default Switch" | Tee-Object .\Output.txt

if (Get-Content Output.txt | Select-String "Installed version:") {
    Copy-Item .\Output.txt .\Output-Current.txt

    $vsVersion = $($(Get-Content Output-Current.txt | Select-String "Installed version:") -Split ":")[-1]
    $currentTag = "$vcEdition-$baseImage-$vsVersion"
    docker tag "vctools:$latestTag" "vctools:$currentTag"
    docker tag vctools:$latestTag philippeqc/vctools:$currentTag
    docker tag vctools:$latestTag philippeqc/vctools:$latestTag

    docker push philippeqc/vctools:$currentTag
    docker push philippeqc/vctools:$latestTag
}
```
