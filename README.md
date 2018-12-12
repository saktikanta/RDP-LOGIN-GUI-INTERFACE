# RDP-LOGIN-GUI-INTERFACE
loadbalancer rdp server

<h1 align="center">
  <br>
  <img alt="FaceID" title="FaceID" src="https://github.com/xames3/FaceID/blob/repository_assets/PopShot_XA_logo_small.png?raw=true" width="250"></a>
  <br>
  RDP GUI Interface
  <br>
</h1>

## Directory structure given below
```
├───open_login_gui.hta
└───config
    ├───env_config.ps1
    ├───rdp_auto_login.ps1
    ├───Images
    |	└───img19.jpg
    └───Logs
```
## Create LOGIN GUI using .hta application
```html
<!--intWindowStyle		Description
0					Hides the window and activates another window.
1					Activates and displays a window. If the window is minimized or maximized, the system restores it to its original size and position. An application should specify this flag when displaying the window for the first time.
2					Activates the window and displays it as a minimized window. 
3					Activates the window and displays it as a maximized window. 
4					Displays a window in its most recent size and position. The active window remains active.
5					Activates the window and displays it in its current size and position.
6					Minimizes the specified window and activates the next top-level window in the Z order.
7					Displays the window as a minimized window. The active window remains active.
8					Displays the window in its current state. The active window remains active.
9					Activates and displays the window. If the window is minimized or maximized, the system restores it to its original size and position. An application should specify this flag when restoring a minimized window.
10					Sets the show-state based on the state of the program that started the application.
<html> 
<head> 
<meta http-equiv="Content-Type" content="text/html; charset=windows-1252">  
<META HTTP-EQUIV="MSThemeCompatible" CONTENT="Yes">  
-->
<html> 
<head>
<title>RDP Login interface</title>
    <HTA:APPLICATION
      APPLICATIONNAME="Test"
      ID="Test"
      VERSION="1.0"/>
<script language="vbscript">  
dim TimerID, scriptlog

Sub RefreshLog 
   Dim fso, file, text
   Set fso  = CreateObject("Scripting.FileSystemObject")
   Set file = fso.OpenTextFile(scriptlog, 1, False, True)
   text = file.ReadAll 
   ExampleOutput.value=text
   file.Close
   set fso = Nothing
   set file = Nothing
End Sub

Sub StartSIAL 
   Dim oShell, scriptPath, appCmd, return, usr, pass, envNDmn, admIp, admLgn
   StrRL.disabled="true"
   Set oShell = CreateObject("WScript.Shell") 
   scriptPath = oShell.CurrentDirectory & "\config\rdp_auto_login.ps1"
   scriptlog = oShell.CurrentDirectory & "\config\Logs\Rdp_Login.log"
   usr = UserArea.value
   pass = PasswordArea.value
   envNDmn = envDmn.value
   admIp = dropdown2.value
   If checkbox1.Checked Then
      admLgn = "yes"
   Else
      admLgn = "no"
   End If
   appCmd = "powershell echo 'Start Processing...' > " & scriptlog
   return = oShell.Run(appCmd, 0, true)
   RefreshLog 
   TimerID = window.setInterval("RefreshLog",1000)
   appCmd = "powershell -executionpolicy bypass &'" & scriptPath & "' '" & envNDmn & "' '" & usr & "' '" & pass & "' '" & admLgn & "' '" & admIp & "' "
   oShell.Run appCmd, 0, true
   StrRL.disabled="false"
End Sub

Sub adminlogin1
   If dropdown2.style.visibility="visible" Then
      dropdown2.style.visibility="hidden"
   Else
      dropdown2.style.visibility="visible"
   End If
End Sub

Sub PopulateServerList
   Dim bbp, lbg, ppd, test, dev, var
   var = envDmn.Value
   prod = Array("ALL", "172.0.0.1", "172.0.0.2", "172.0.0.4", "172.0.0.5")
   dr = Array("ALL", "172.0.0.7", "172.0.0.8")
   ppd = Array("ALL", "172.0.0.16", "172.0.0.17", "172.0.0.18", "172.0.0.19", "172.0.0.21", "172.0.0.22")
   test = Array("ALL", "172.0.0.74", "172.0.0.75", "172.0.0.96", "172.0.0.97", "172.0.0.13", "172.0.0.14")
   dev = Array("ALL", "172.0.0.24", "172.0.0.25", "172.0.0.26", "172.0.0.27", "172.0.0.28", "172.0.0.29", "172.0.0.30")

   ExampleOutput.value=var
   
   For Each opt in dropdown2.Options
       opt.RemoveNode
   Next 

   If var="prod\domain1" Then
      For Each ips in prod
         Set opt = document.createElement("option")
	     opt.Value = CStr(ips)
         opt.Text = CStr(ips)
         dropdown2.Add(opt)
      Next    
   Elseif var="dr\domain1" Then
      For Each ips in dr
         Set opt = document.createElement("option")
         opt.Value = CStr(ips)
         opt.Text = CStr(ips)
         dropdown2.Add(opt)
      Next    
   Elseif var="PREPROD\domain2" Then
      For Each ips in ppd
         Set opt = document.createElement("option")
         opt.Value = CStr(ips)
         opt.Text = CStr(ips)
         dropdown2.Add(opt)
      Next    
   Elseif var="TEST\domain3" Then
      For Each ips in test
         Set opt = document.createElement("option")
         opt.Value = CStr(ips)
         opt.Text = CStr(ips)
         dropdown2.Add(opt)
      Next    
   Elseif var="DEV\domain3" Then
      For Each ips in dev
         Set opt = document.createElement("option")
         opt.Value = CStr(ips)
         opt.Text = CStr(ips)
         dropdown2.Add(opt)
      Next    
   End If
End Sub

Sub Initialize() 
   window.resizeTo 700,565
   PopulateServerList
End Sub


</script>

<style type="text/css"> 
body { 
   font-family: Tahoma; 
   font-size: 15px;
   background-image: url("config/Images/img19.jpg");
   background-color: #151B54;
   color: #FFFFFF;
}

pre {
   font-family: Tahoma;
}

</style>
<hta:application>
</head> 
<body onload="Initialize()">
<pre><B>   Atos Terminal Login</B>

   UserID <input type="text" name="UserArea" size="15">   Password <input type="password" name="PasswordArea" size="15">
   
                   Environment \ Domain <select size="1" name="envDmn" onchange="PopulateServerList"><option value="BBP\RMGP">BBP\RMGP</option><option value="LBG\RMGP">LBG\RMGP</option><option value="PREPROD\RMGV">PREPROD\RMGV</option><option value="TEST\RMGN">TEST\RMGN</option><option value="DEV\RMGN">DEV\RMGN</option></select>     <button type="button" id="StrRL" onclick="StartSIAL()"><b>Login</b></button>
				   
                                  Termonal Server <select name="dropdown2" id="dropdown2"><option value="nill">-- Select Server --</option></select>    <input type="checkbox" name="checkbox1"> Admin login<!-- onclick="adminlogin1()">-->
									   
                                                         
   </pre>
   <button style="color:#FFFFFF;background-color:#5E610B" onclick="RefreshLog()"><b>Refresh</button> status log <span id="disp_path"></span>
   <textarea id="ExampleOutput" style="width:100%" rows="15"></textarea>
</body> 
</html>
```

## Env_config file for this is:
```powershell
Param($ScriptDir, $FileNameStartWitn, $envdomn, $inputUid, $inputPas, $bbpLbg)

#-----------------------------------
# I/O and LOG file settings
#-----------------------------------
$logDir = $ScriptDir + "\Logs"
$initial = $FileNameStartWitn #"Inc_Active_List"
$logfile = $logDir + "\" + $initial + ".log"
if(!(Test-Path $logDir)) { md $logDir }
echo 'Start Processing...' > $logfile

#-----------------------------------
# Get the registry settings
#-----------------------------------
# import assembly for keyboard input
Add-Type -AssemblyName Microsoft.VisualBasic, System.Web, System.Windows.Forms


#=====================================
# Function to append output in log file
#=====================================
function log($string, $color)
{
   if ($Color -eq $null) {$color = "white"}
   write-host $string -foregroundcolor $color
   $string | out-file -Filepath $logfile -append
}

$rEnv=$envdomn.split('\')[0]
$domn=$envdomn.split('\')[1]
log "$rEnv Environment"
log "$domn Domain"

$usrFile = $ScriptDir + '\' + $domn + "UserID.txt"
$passFile = $ScriptDir + '\' + $domn + "EncPass.txt"
$snusr = $( cat $usrFile 2> $null )
$snpass = $( cat $passFile 2> $null)

If ($inputUid -eq $Null -OR $inputUid -eq '') {
  If ($snusr -eq $Null) {
    Log "First time login? Please provide your UserID and try again."
    $exit_flg=1
    exit
  }
}else {
  echo "$domn\$inputUid" | out-file -Filepath $usrFile
  $snusr = $domn + '\' + $inputUid
}

If ($inputPas -eq $Null -OR $inputPas -eq '') {
  If ($snpass -eq $Null) {
    Log "First time login? Please provide your Password and try again."
    $exit_flg=1
    exit
  }
}else {
  $qvpas = ConvertTo-SecureString $inputPas -AsPlainText -Force
  $snpass = ConvertFrom-SecureString $qvpas
  $snpass | out-file -Filepath $passFile
}
$secPass = $snpass | ConvertTo-SecureString

$sv_time = $( (Get-Date).tostring("HHmmss") )
# Below setting is required for html entuty decoding
Add-Type -AssemblyName System.Web

#-----------------------------------
# Variables sets to limit multithreading
#-----------------------------------
$thread_count = 0 
$SleepTimer = 500
$MaxThreads = 7 # Setting up maxmimun threads

#-----------------------------------
# Server list
#-----------------------------------
   prod = Array("ALL", "172.0.0.1", "172.0.0.2", "172.0.0.4", "172.0.0.5")
   dr = Array("ALL", "172.0.0.7", "172.0.0.8")
   ppd = Array("ALL", "172.0.0.16", "172.0.0.17", "172.0.0.18", "172.0.0.19", "172.0.0.21", "172.0.0.22")
   test = Array("ALL", "172.0.0.74", "172.0.0.75", "172.0.0.96", "172.0.0.97", "172.0.0.13", "172.0.0.14")
   dev = Array("ALL", "172.0.0.24", "172.0.0.25", "172.0.0.26", "172.0.0.27", "172.0.0.28", "172.0.0.29", "172.0.0.30")

$prod_dr=$proddr

$prod_donain1=(
"172.0.0.1",
"172.0.0.2",
"172.0.0.4",
"172.0.0.5"
)

$dr_donain1=(
"172.0.0.7",
"172.0.0.8"
)           
            
$preprod_donain2=(
"172.0.0.16",
"172.0.0.17",
"172.0.0.18",
"172.0.0.19",
"172.0.0.21",
"172.0.0.22"
)

$test_donain3=(
"172.0.0.74",
"172.0.0.75",
"172.0.0.96",
"172.0.0.97",
"172.0.0.13",
"172.0.0.14"
)

$dev_donain3=(
"172.0.0.24",
"172.0.0.25",
"172.0.0.26",
"172.0.0.27",
"172.0.0.28",
"172.0.0.29",
"172.0.0.30"
)

#=====================================
# Function to connect rdp
#=====================================
# helper function to locate a open program using by a given Window name
Function FindWindow([string]$windowName, [int]$sleepInterval = 1000) {
  
  [int]$currentTry = 0;
  [bool]$windowFound = $false;
  
  Do {
    Start-Sleep -Milliseconds $sleepInterval
    Try {
	    [Microsoft.VisualBasic.Interaction]::AppActivate($windowName)
      $windowFound = $true;
    } Catch {
      $windowFound = $false;
    }
	$currentTry++;
    if ( $currentTry -ge "15" )
    {
	  Log "Exit after trying for long time to get the RDP session."
      break;
    }
  } While ($windowFound -eq $false)
  return $windowFound;
}

Function rdp_login($ipAdr, $admLn) {
    cmdkey.exe /generic:$ipAdr /user:$snusr /pass:$( [Runtime.InteropServices.Marshal]::PtrToStringAuto([Runtime.InteropServices.Marshal]::SecureStringToBSTR($secPass)) )
    if ( $admLn -eq 'yes' )
    {
        Log "Admin login to $ipAdr"
        mstsc.exe /v $ipAdr /admin /f
    } else {
        mstsc.exe /v $ipAdr /f
    }
}

$closeRemain=$false
#-----------------------------------
# Get the rdp list firsr
#-----------------------------------
$PrevOpendRdpPidList=(Get-WMIObject -Class Win32_Process -Filter "Name='mstsc.exe'" | where { $_.WorkingSetSize -ge 40000000 }).Handle

function connect_rdp($svr_list, $admLogin)
{
    if ( $snusr.split('\')[0] -eq "donain1" )
    {
        $svr_list | % {
            rdp_login $_ $admLogin
        }
    } else {
        $svr_list | % {
            rdp_login $_ $admLogin
		}
		$svr_list | % {
			if($closeRemain -eq $true) {
				break;
			}elseif(FindWindow("Windows Security")) {
				$attempts = 1
				while ($True) {
				    if($attempts -eq 3 ) {
						break;
					}elseif((Get-WMIObject -Class Win32_Process -Filter "Name='mstsc.exe'" | where { $_.WorkingSetSize -ge 40000000 -AND $PrevOpendRdpPidList -notcontains $_.Handle })) {
						stop-process (Get-WMIObject -Class Win32_Process -Filter "Name='mstsc.exe'" | where { $_.WorkingSetSize -lt 40000000 -AND $PrevOpendRdpPidList -notcontains $_.Handle }).Handle
						$closeRemain=$true
						log "End time : $(Get-Date)"
						break;
					}
					try {
						Start-Sleep -Milliseconds 1000
						[Microsoft.VisualBasic.Interaction]::AppActivate("Windows Security")
						Start-Sleep -Milliseconds 500
						[System.Windows.Forms.SendKeys]::SendWait($( [Runtime.InteropServices.Marshal]::PtrToStringAuto([Runtime.InteropServices.Marshal]::SecureStringToBSTR($secPass)) )+'{ENTER}')
						log "$attempts Attempt to login.."
					} catch {
						log "$attempts Attempt to login failed seems RDP not yet opened"
						start-sleep 1
					}
					$attempts++
				}
            }
        }
    }  
}

function remove_cr($svr_list)
{
	$svr_list | % {
		cmdkey.exe /delete:$_
	}
}

function enter_into_rdp($svr_name)
{
	if(FindWindow("$svr_name - Remote Desktop Connection")) {
		Start-Sleep -Milliseconds 250
        Log "rdp found"
        [System.Windows.Forms.SendKeys]::SendWait('{ENTER}')
    }
}
#get-process iexplore | stop-process
```
## Powershell script to invoke rdp
```powershell
Param($envdomn, $usr, $pass, $admLogin, $ipAdr, $bbpLbg)
# If you are getting the error .ps1 script cannot be run. Then execute the below command.
#Set-ExecutionPolicy -Scope CurrentUser -ExecutionPolicy Unrestricted -Force

$ScriptDir = "$PSScriptRoot" # Script path
$EnvConfig = $ScriptDir + "\env_config.ps1"
$FileNameStartWitn = "Rdp_Login"

#=====================================
# Environment Variable Configuration
#=====================================
. $EnvConfig $ScriptDir $FileNameStartWitn $envdomn $usr $pass $bbpLbg

if($exit_flg -eq 1) {
  log "END"
  exit
}

log "Log file is: $logfile"

log "Start time : $(Get-Date)"
log "Domain\UserID : $snusr"

if( $domn -eq "domain1" ) {
  if( $rEnv -eq "prod" ) {
    if( $ipAdr -eq "ALL" ) {
	  $rmv_crd = $prod_domain1
    }
	else {
	  $rmv_crd = $ipAdr
	}
  }
  else {
    if( $ipAdr -eq "ALL" ) {
	  $rmv_crd = $dr_domain1
    }
	else {
	  $rmv_crd = $ipAdr
	}
  }
}
elseif( $domn -eq "domain2" ) {
   if( $ipAdr -eq "ALL" ) {
     $rmv_crd = $preprod_domain2
   }
   else {
     $rmv_crd = $ipAdr
   }
}
elseif( $domn -eq "domain3" ) {
  if( $rEnv -eq "TEST" ) {
    if( $ipAdr -eq "ALL" ) {
	  $rmv_crd = $test_domain3
    }
	else {
	  $rmv_crd = $ipAdr
	}
  }
  else {
    if( $ipAdr -eq "ALL" ) {
	  $rmv_crd = $dev_domain3
    }
	else {
	  $rmv_crd = $ipAdr
	}
  }
}

connect_rdp $rmv_crd $admLogin

if($closeRemain -eq $true) {
	$host.Exit()
}
$count=20
while($Count -ge 0 ) {
    log "$count seconds"
	if((Get-WMIObject -Class Win32_Process -Filter "Name='mstsc.exe'" | where { $_.WorkingSetSize -ge 40000000 -AND $PrevOpendRdpPidList -notcontains $_.Handle })) {
		stop-process (Get-WMIObject -Class Win32_Process -Filter "Name='mstsc.exe'" | where { $_.WorkingSetSize -lt 40000000 -AND $PrevOpendRdpPidList -notcontains $_.Handle }).Handle
		$svr_name = ((Get-WMIObject -Class Win32_Process -Filter "Name='mstsc.exe'" | where { $_.WorkingSetSize -ge 40000000 -AND $PrevOpendRdpPidList -notcontains $_.Handle }).CommandLine -split ' ')[2]
		break;
	}
	else {
		$Count--
	}
	Start-Sleep -Seconds 1
}

if ( $svr_name ) {
    log "Now entering into the rdp" 
    enter_into_rdp $svr_name
}

remove_cr $rmv_crd

log "End time : $(Get-Date)"

#=====================================
# End
#=====================================

```

