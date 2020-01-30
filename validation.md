
Module 1:- Create a virtual machine


```
$rg = "ODL-validation-143815"

$vms = Get-AzureRmVM  -ResourceGroupName $rg 
if ($vms.Count -eq 0) 
{ 
     $message = @{Status ="Failed"; Message = "No VMs found in $rg"}| ConvertTo-Json 
  $message
} 
else 
{ 
    ForEach ($vm in $vms) 
   
 { 
    $vmSize = $vms.hardwareprofile.VmSize 
    $vmImageOffer =  $vms.StorageProfile.ImageReference.Offer 
 	 	 	 
 
        if($vmSize -like "Standard_D2s_v3"-or $vmImageOffer -like "Windows Server 2019 Datacenter" ) 
        { 
         $message = @{Status ="Succeeded"; Message = "VM found that matches the size Standard_DS2_v3 on Windows Server 2019 Datacenter"}| ConvertTo-Json 
         $message
break 
       
 }                                 else 
 
      { 
                $message = @{Status ="Failed"; Message = "No VM found with size Standard_DS2_v3 on Windows Server 2019 Datacenter"}| ConvertTo-Json
 
                $message
      } 
    } 
} 
 ```
