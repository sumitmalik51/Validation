
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
 
 
 
 
 Create Module 2:- Create A Container Instances:-
 
 ```
 $rg = "ODL-validation-143815" 
Get-AzureRmContainerGroup 
$container = Get-AzureRmContainerGroup  -ResourceGroupName $rg 
if ($container.Count -eq 0) 
{ 
     $message = @{Status ="Failed"; Message = "No Container Instance found that matches the OStype Linux and container image is microsoft/aci-helloworld $rg"}| ConvertTo-Json 
  $message
} 
else 
{ 
    ForEach ($cont in $container) 
    { 
    $Image = $container.Containers.Image 
    $containerOffer =  $container.OsType 
      $containerDNS = $container.Fqdn 
 	 	 	 
 
        if($containerOffer -like "Linux" -and $Image -like "microsoft/acihelloworld" -and $containerDNS.EndsWith('azurecontainer.io')) 
        { 
                $message = @{Status ="Succeeded"; Message = "Container Instance found that matches the OStype as Linux and container image is microsoft/acihelloworld and having DNS name enabled"}| ConvertTo-Json 
         
                $message                 
                break 
        }       
         else 
        { 
                $message = @{Status ="Failed"; Message = "No Container Instance found that matches the OStype Linux and container image is microsoft/acihelloworld"}| ConvertTo-Json 
                $message 
        } 
    } 
} 
 
```


Create 3:- Create a Vnet witn two virtual Machines:-

```
$rg = "ODL-validation-143815" 
 
$vms = Get-AzureRmVM  -ResourceGroupName $rg 
$vnet = get-AzureRmVirtualNetwork -ResourceGroupName $rg 
 
 
  
if ($vms.Count -eq 0) 
{ 
     $message = @{Status ="Failed"; Message = "No VM found with one vnet in $rg resource group $rg"}| ConvertTo-Json 
    $message 
} 
else 
{ 
    ForEach ($vm in $vms) 
    { 
    
    $vmImageOffer =  $vms.StorageProfile.ImageReference.Offer 
 	 	 	 
 
        if($vmImageOffer -like "WindowsServer" -and $vnet.Count -eq 1 -and 
$vms.Count -eq 2) 
        { 
                $message = @{Status ="Succeeded"; Message = "Two VM found with one vnet in $rg resource group"}| ConvertTo-Json 
         
                $message                 
                break 
        }       
        else 
        { 
                $message = @{Status ="Failed"; Message = "No VM found with one vnet in $rg resource group"}| ConvertTo-Json 
       $message 
        } 
    } 
} 
```


Create a blob storage:-

```
$rg = "ODL-validation-143815" 
Get-AzureRmStorageAccount 
$storage = Get-AzureRmStorageAccount  -ResourceGroupName $rg if ($storage.Count -eq 0) 
{ 
     $message = @{Status ="Failed"; Message = "No Storage account found in the resource group $rg"}| ConvertTo-Json 
   $message 
   }
    else 
{ 
    ForEach ($sto in $storage) 
    { 
    $Accesstier = $storage.AccessTier 
    $Kind =  $storage.Kind 
      $Sku = $storage.Sku.Name 
 	 	 	 
 
        if($Accesstier -like "Hot" -and $Kind -like "StorageV2" -and $Sku-like "StandardLRS") 
        { 
                $message = @{Status ="Succeeded"; Message = "Storage Account found with Hot Access tier and StorageV2 kind and Standard_LRS SKU"}| ConvertTo-Json 
         
                $message        
                break 
        }                                 
        else 
       
        { 
                $message = @{Status ="Failed"; Message = "NO Storage Account found with Hot Access tier and StorageV2 kind and Standard_LRD SKU"}| ConvertTo-Json       
                 $message 
        } 
    } 
} 
```
