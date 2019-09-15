<img src="https://i.imgur.com/MiKnaJL.png">
<h2>Win10 system.NET Service runtime optimization of CPU how to solve.</h2>

<ul>
  <li>In the process of using Windows 10 system, it was found that a process called <b>.NET Runtime Optimization Service</b> takes up high CPU usage.</li>
  <li>What should I do? What is the process of the .NET runtime optimization service?</li>
  <li>In general, when you install .NET, the program will appear on the computer and .NET will cause speed of use when updated. It will automatically turn off after a few minutes.</li>
  <li>If it is occupied by CPU usage, it may be because some high priority orders are made as soon as possible and other low priority jobs work when the computer is idle.</li>
  <li>The cause of the CPU delay is stuck in the low priority work. Next, let's see the solution to the problem with <b>Xiaobian</b>!</li>
</ul>
<hr>
<h2>Solution</h2>
<ul>
  <li>Press <b>Windows + Q</b> to search for "<b>PowerShell</b>" either right in the Start menu or by tapping the search button right next to it. Right-click on the first result which appears at the top and select the "<b>Run as Administrator</b>" option.</li>
<br/>
  <img src="https://i.imgur.com/grV7hII.png">
<br/><br/>
<li>Enter the following code: <a href="https://raw.githubusercontent.com/duyplus/Fix-High-CPU-Usage-by-.NET-Runtime-Optimization-Service/master/code.txt" target="_blank"><b>Click here</b></a> or copy below.
</li></ul><br/>
<blockquote style="-webkit-text-stroke-width: 0px; background: rgb(232, 249, 244); border: 1px solid rgb(142, 227, 200); box-sizing: border-box; clear: right; color: #181818;font-style: normal; font-variant-caps: normal; font-variant-ligatures: normal; font-weight: 300; letter-spacing: normal; line-height: 1.6em; margin: 1.5em 0px; orphans: 2; padding: 1.6em; text-align: start; text-decoration-color: initial; text-decoration-style: initial; text-indent: 0px; text-transform: none; white-space: normal; widows: 2; word-spacing: 0px;">
# Script to force the .NET Framework optimization service to run at maximum speed.<br/>
$isWin8Plus = [Environment]::OSVersion.Version -ge (new-object 'Version' 6,2)<br/>
$dotnetDir = [environment]::GetEnvironmentVariable("windir","Machine") + "\Microsoft.NET\Framework"<br/>
$dotnet2 = "v2.0.50727"<br/>
$dotnet4 = "v4.0.30319"<br/>
$dotnetVersion = if (Test-Path ($dotnetDir + "\" + $dotnet4 + "\ngen.exe")) {$dotnet4} else {$dotnet2}<br/>
$ngen32 = $dotnetDir + "\" + $dotnetVersion +"\ngen.exe"<br/>
$ngen64 = $dotnetDir + "64\" + $dotnetVersion +"\ngen.exe"<br/>
$ngenArgs = " executeQueuedItems"<br/>
$is64Bit = Test-Path $ngen64<br/>
#32-bit NGEN -- appropriate for 32-bit and 64-bit machines<br/>
Write-Host("Requesting 32-bit NGEN") <br/>
Start-Process -wait $ngen32 -ArgumentList $ngenArgs<br/>
#64-bit NGEN -- appropriate for 64-bit machines<br/>
if ($is64Bit) {<br/>
&nbsp;&nbsp;&nbsp;&nbsp;Write-Host("Requesting 64-bit NGEN") <br/>
&nbsp;&nbsp;&nbsp;&nbsp;Start-Process -wait $ngen64 -ArgumentList $ngenArgs<br/>
}<br/>
#AutoNGEN for Windows 8+ machines<br/>
if ($isWin8Plus) {<br/>
&nbsp;&nbsp;&nbsp;&nbsp;Write-Host("Requesting 32-bit AutoNGEN -- Windows 8+") <br/>
&nbsp;&nbsp;&nbsp;&nbsp;schTasks /run /Tn "\Microsoft\Windows\.NET Framework\.NET Framework NGEN v4.0.30319"<br/>
}<br/>
#64-bit AutoNGEN for Windows 8+ machines<br/>
if ($isWin8Plus -and $is64Bit) {<br/>
&nbsp;&nbsp;&nbsp;&nbsp;Write-Host("Requesting 64-bit AutoNGEN -- Windows 8+") <br/>
&nbsp;&nbsp;&nbsp;&nbsp;schTasks /run /Tn "\Microsoft\Windows\.NET Framework\.NET Framework NGEN v4.0.30319 64"<br/>
}<br/>
</blockquote>
<br/>
<ul><li>Then you can check whether .NET Runtime Optimization Service high CPU usage windows 10 still appears.</li></ul>
  <img src="https://i.imgur.com/6kOlV49.jpg">
Thank you for watching and good luck.
<h3>Info</h3>
<ul>
  <li>Facebook: <a href="https://www.facebook.com/duyplusx">@duyplusx</a></li>
  <li>Instagram: <a href="https://www.instagram.com/nghdyyy">@nghdyyy</a></li>
  <li>Twitter: <a href="https://twitter.com/duyplusdz">@duyplusdz</a></li>
