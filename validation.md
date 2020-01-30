
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


Create a SQL Database:-



```
$rg = "ODL-validation-143815" 
Get-AzureRmSqlServer 
$Name = $SQL.ServerName 
$SQL = Get-AzureRmSqlServer  -ResourceGroupName $rg 
$SQLDB = Get-AzureRmSqlDatabase -ServerName $Name -ResourceGroupName $rg 
 
if ($SQLDB.Count -eq 0) 
{ 
     $message = @{Status ="Failed"; Message = "No SQL Database found in the resource group $rg"}| ConvertTo-Json 
   $message 
   } 
   else 
{ 
    ForEach ($SQLD in $SQLDB) 
    { 
        $type = $SQLDB.DatabaseName 
        $Length = $SQLDB.Length 
 
        
 	 	 	 
 
        if($type -like "master" -and $Length -like "1") 
        { 
                $message = @{Status ="Succeeded"; Message = "SQL Server found with database name master"}| ConvertTo-Json 
         
                $message              
                   break 
        }       
          else 
        { 
                $message = @{Status ="Failed"; Message = "No SQL Server found with database name master"}| ConvertTo-Json 
       $message 
        } 
    } 
}  
   
   
```


Create a IotHub:-


```
$rg = "ODL-validation-143815" 
Get-AzureRmIotHub 
$iot = Get-AzureRmIotHub  -ResourceGroupName $rg 
 
if ($iot.Count -eq 0) 
{ 
     $message = @{Status ="Failed"; Message = "No IotHub Devices found in the resource group $rg"}| ConvertTo-Json 
   $message 
   }
    else 
{ 
    ForEach ($io in $iot) 
    { 
       
       $SKU = $iot.Sku.Name 
        
 	 	 	 
 
        if($SKU -like "F1") 
        { 
                $message = @{Status ="Succeeded"; Message = "IotHub Devices found  with Free Tier"}| ConvertTo-Json 
         
                $message        
                break 
        }       
           else 
        { 
                $message = @{Status ="Failed"; Message = "No IotHub Devices found with Free Tier "}| ConvertTo-Json 
       $message 
        } 
    } 
} 
```



Create a webapp:-


```
$rg = "ODL-validation-143183" 
 
$webapp = Get-AzureRmWebApp -ResourceGroupName $rg 
$Sitename = $webapp.SiteName 
if ($webapp.Count -eq 0) 
{ 
     $message = @{Status ="Failed"; Message = "No Web App found in the resource group $rg"}| ConvertTo-Json 
   $message 
   }
    else 
{ 
    ForEach ($webap in $webapp ) 
    {
       
       $Capacity = $webapp.Capacity 
        	 
 
        if($Capacity -eq "4") 
        { 
                $message = @{Status ="Succeeded"; Message = "Web App in the resource group $rg"}| ConvertTo-Json 
         
                $message               
                  break 
        }       
         else 
        { 
                $message = @{Status ="Failed"; Message = "No Web App found in the resource group $rg"}| ConvertTo-Json 
       $message 
        } 
    } 
} 
``` 


Secure Network Traffic:-


```
           
$rg = "ODL-validation-143815"
$vms = Get-AzureRmVM  -ResourceGroupName $rg 
 $nsg =  Get-AzureRmNetworkSecurityGroup -ResourceGroupName $rg
  $nsgname = $nsg.Name

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
     $nsgname = $nsg.Name
 	 	 	 
 
        if($vmSize -like "Standard_D2s_v3"-and $vmImageOffer -like "WindowsServer" -and $nsgname -eq "myNSGSecure") 
        { 
                $message = @{Status ="Succeeded"; Message = "VM found that matches the size Standard_DS2_v3 on Windows Server 2019 Datacenter"}| ConvertTo-Json 
         $message
          break 
       
 }                                 
  else 
 
      { 
                $message = @{Status ="Failed"; Message = "No VM found with size tandard_DS2_v3 on Windows Server 2019 Datacenter"}| ConvertTo-Json
 
              $message
                      } 
    } 
} 
```



Create a key vault:-



```
$rg = "ODL-validation-143183" 
  
$key = Get-AzureRmKeyVault -ResourceGroupName $rg 
 
$Vaultname = $key.VaultName 
$secret = Get-AzureKeyVaultSecret -VaultName $Vaultname 
 
 
$secretname = $secret.Name
 if ($key.Count -eq 0) 
{ 
     $message = @{Status ="Failed"; Message = "No Key Vault found in the resource group $rg"}| ConvertTo-Json 
   $message
    } 
   else 
{ 
    ForEach ($secre in $secret) 
    { 
       
             
 
        if($secretname.Count -eq "1" -and $secretname -like "ExamplePassword") 
        { 
                $message = @{Status ="Succeeded"; Message = "Found Key Vault with one secret in the resource group $rg"}| ConvertTo-Json 
         
                $message            
                     break 
        }                       
                  else 
        { 
                $message = @{Status ="Failed"; Message = "No Key Vault found with secret in the resource group $rg"}| ConvertTo-Json 
       $message 
        } 
    } 
} 
```
 

Create a allowed policy:-



```
$rg = "ODL-validation-143183" 
  
$policy = Get-AzureRmPolicyDefinition -Name Allowed locations 
 
 
  
if ($policy.Count -eq 0) 
{ 
     $message = @{Status ="Failed"; Message = "Not Found Allowed locations policy in the resource group $rg"}| ConvertTo-Json 
   $message
    }
    else 
{ 
    ForEach ($polic in $policy) 
    { 
      $name = $policy.Name 
            
        if($name -like "Allowed locations") 
        { 
                $message = @{Status ="Succeeded"; Message = "Found Allowed locations policy in resource group $rg"}| ConvertTo-Json 
         
                $message               
                  break 

        }                    
           else 
        { 
                $message = @{Status ="Failed"; Message = "Not Found Allowed locations policy in resource group $rg"}| ConvertTo-Json 
       $message 
        } 
    } }  
 
```


Create a ROle:-



```
$rg = "ODL-validation-143183" 

 
$Role = Get-AzureRmRoleAssignment -ResourceGroupName $rg -RoleDefinitionName "Virtual Machine Contributor"  
 
 
 
  
if ($Role.Count -eq 0) 
{ 
     $message = @{Status ="Failed"; Message = "No Role Assignment is found in the resource group $rg"}| ConvertTo-Json 
   $message
    }
     else 
{ 
    ForEach ($Rol in $Role) 
    { 
       
            $Rolename = $Role.RoleDefinitionName 
 
        if($Rolename -like "Virtual Machine Contributor") 
        { 
                $message = @{Status ="Succeeded"; Message = "Found Virtual Machine Contributor Role in resource group $rg"}| ConvertTo-Json 
         
                $message 
                break 
        }                                 
        else 
        { 
                $message = @{Status ="Failed"; Message = "No Found Virtual Machine Contributor Role in resource group $rg"}| ConvertTo-Json 
                $message 
        } 
    } 
} 
  
```

Create a resource lock:-



```

$rg = "ODL-validation-143183" 

 
$lock = Get-AzureRmResourceLock -ResourceGroupName $rg 
 
$lockname = $lock.Name 
$type = $lock.Properties 
 
 
  
if ($lock.Count -eq 0) 
{ 
     $message = @{Status ="Failed"; Message = "No RG Lock found in the resource group $rg"}| ConvertTo-Json 
   $message 
   } 
   else 
{ 
    ForEach ($loc in $lock) 
    { 
       
             
 
        if($lockname -like "RGLock" -and $type.level -eq "CanNotDelete" ) 
        { 
                $message = @{Status ="Succeeded"; Message = "Found Key Vault with one secret in the resource group $rg"}| ConvertTo-Json          
                $message              
                   break 
        }                         
                else 
        { 
                $message = @{Status ="Failed"; Message = "No Key Vault found with secret in the resource group $rg"}| ConvertTo-Json 
       $message 
        } 
    } 
} 
```

Create required tag:- 




```
$rg = "ODL-validation-143183" 

 
$policy = Get-AzureRmPolicyDefinition -Name Require specified tag 
 
 
  
if ($policy.Count -eq 0) 
{ 
     $message = @{Status ="Failed"; Message = "Not Found Require specified tag  policy in the resource group $rg"}| ConvertTo-Json 
   $message
    } 
    else 
{ 
    ForEach ($polic in $policy) 
    { 
      $name = $policy.Name 
            
        if($name -like "Require specified tag") 
        { 
                $message = @{Status ="Succeeded"; Message = "Found Require specified tag policy in resource group $rg"}| ConvertTo-Json 
         
                $message                
                 break 
        }                               
          else 
        { 
                $message = @{Status ="Failed"; Message = "Not Found Require specified tag policy in resource group $rg"}| ConvertTo-Json 
       $message 
        } 
    } }  
``` 


