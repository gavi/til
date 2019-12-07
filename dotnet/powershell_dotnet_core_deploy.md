# Powershell deploy of dotnet core application on IIS

If you need to deploy a running application on dotnet core with IIS, you need to copy a app_offline.htm file in the root of application.
This will gracefully shutdown the iis process hosting the dotnet core app

Here is an example

```powershell
echo "offline" > c:\web\web_path\app_offline.htm
Start-Sleep -s 15
Remove-Item  C:\web\web_path\* -Recurse -Force
$fileName = gci E:\stage\stagedfiles\*.zip |sort LastWriteTime| select -last 1
Expand-Archive $fileName -DestinationPath C:\web\web_path\ -Force
```

In the above example, a file is created and placed in the web root folder. Then the powershell waits for 15 seconds(The default kill time is 10 seconds) and then
unarchives the files.

This will make sure none of the files are locked when trying to copy.
