In the process of using Windows 10 system, it was found that a process called .NET Runtime Optimization Service takes up high CPU usage.

What should I do? What is the process of the .NET runtime optimization service? In general, when you install .NET, the program will appear on the computer and .NET will cause speed of use when updated.

It will automatically turn off after a few minutes. If it is occupied by CPU usage, it may be because some high priority orders are made as soon as possible and other low priority jobs work when the computer is idle.

The cause of the CPU delay is stuck in the low priority work. Next, let's see the solution to the problem with Xiaobian!


1. Press Windows + Q to search for PowerShell, right-click on Windows PowerShell and select [Run as Administrator];
2. Enter the following code:

# Script to force the .NET Framework optimization service to run at maximum speed.
$isWin8Plus = [Environment]::OSVersion.Version -ge (new-object 'Version' 6,2)
$dotnetDir = [environment]::GetEnvironmentVariable("windir","Machine") + "\Microsoft.NET\Framework"
$dotnet2 = "v2.0.50727"
$dotnet4 = "v4.0.30319"
$dotnetVersion = if (Test-Path ($dotnetDir + "\" + $dotnet4 + "\ngen.exe")) {$dotnet4} else {$dotnet2}
$ngen32 = $dotnetDir + "\" + $dotnetVersion +"\ngen.exe"
$ngen64 = $dotnetDir + "64\" + $dotnetVersion +"\ngen.exe"
$ngenArgs = " executeQueuedItems"
$is64Bit = Test-Path $ngen64
#32-bit NGEN -- appropriate for 32-bit and 64-bit machines
Write-Host("Requesting 32-bit NGEN") 
Start-Process -wait $ngen32 -ArgumentList $ngenArgs
#64-bit NGEN -- appropriate for 64-bit machines
if ($is64Bit) {
    Write-Host("Requesting 64-bit NGEN") 
    Start-Process -wait $ngen64 -ArgumentList $ngenArgs
}
#AutoNGEN for Windows 8+ machines
if ($isWin8Plus) {
    Write-Host("Requesting 32-bit AutoNGEN -- Windows 8+") 
    schTasks /run /Tn "\Microsoft\Windows\.NET Framework\.NET Framework NGEN v4.0.30319"
}
#64-bit AutoNGEN for Windows 8+ machines
if ($isWin8Plus -and $is64Bit) {
    Write-Host("Requesting 64-bit AutoNGEN -- Windows 8+") 
    schTasks /run /Tn "\Microsoft\Windows\.NET Framework\.NET Framework NGEN v4.0.30319 64"
