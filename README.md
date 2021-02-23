Vlocity ES Tools (Beta)
==============

##### Disclaimer: This tool is not an offical tool from Vlocity or Salesforce. It was created with the intent of distributing certain uitlility tools, Use it at your own risk.


# Install
```sh-session
$ sfdx plugins:install vlocityestools # Requires SFDX-CLI
```

# Usage
```sh-session
$ sfdx COMMAND
running command...

$ sfdx --help [COMMAND]
USAGE
  $ sfdx COMMAND
```

# Commands:

### vlocityestools:clean:epcgkfix
### vlocityestools:clean:omniscripts
### vlocityestools:clean:templates
### vlocityestools:compare:folders
### vlocityestools:clean:datapacks
### vlocityestools:clean:savedomniscripts
### vlocityestools:report:dependencies:local
### vlocityestools:report:dependencies:remote
### vlocityestools:report:activeomniscript
### vlocityestools:sfsource:createdeltapackage
### vlocityestools:sfsource:createdeltapackagelocal
### vlocityestools:sfsource:updatedeltahash
### vlocityestools:clean:calcmatrix
### vlocityestools:login:login
### vlocityestools:clean:objects
### vlocityestools:sfsource:createmocklwcos
  
'    '

# Commands Info:


## vlocityestools:clean:epcgkfix 

Sync Global Keys between two orgs (from source to target org) for the descrived objects. If Duplicates are detected, Global Keys wil be updated. 
Use -v,--check to only check and do not insert the records. 

Notes: 
- For now, this only works for CMT Package. 
- Please read carefully the documentation and check with the EPC implemtation team that this fits your needs and that the match Scenarios do apply for your implementation, Used it at your own risk. 
- Avoid Using it in Production. Create a Full or partial copy if necessary. 

#### Attribute Assignments Scenarios:

##### 1. Non Override Attribute Assignments (IsOverride__c = false )

This will Match Records to Sync the GlobalKeys by using a key generated by the combination of these two Values: 

```
AttributeAssignment.vlocity_namespace__ObjectId__c.vlocity_namespace__GlobalKey__c
AttributeAssignment.vlocity_namespace__AttributeId__r.vlocity_namespace__GlobalKey__c
```

##### 2. Override Attribute Assignments (IsOverride__c = true )

This will query OverideDefinitions by using the considitions:

```
OverrideDefinition.vlocity_namespace__OverrideType__c = 'Attribute' 
OverrideDefinition.vlocity_namespace__OverridingAttributeAssignmentId__c != null 
OverrideDefinition.vlocity_namespace__OverriddenAttributeAssignmentId__c != null 
```

And Match them across source and Target by the combination of these Values: 

```
OverrideDefinition.vlocity_namespace__ProductHierarchyGlobalKeyPath__c
OverrideDefinition.vlocity_namespace__ProductId__r.vlocity_namespace__GlobalKey__c
OverrideDefinition.vlocity_namespace__PromotionId__r.vlocity_namespace__GlobalKey__c
OverrideDefinition.vlocity_namespace__PromotionItemId__r.vlocity_namespace__ProductId__r.vlocity_namespace__GlobalKey__c
OverrideDefinition.vlocity_namespace__PromotionItemId__r.vlocity_namespace__ContextProductId__r.vlocity_namespace__GlobalKey__c
OverrideDefinition.vlocity_namespace__PromotionItemId__r.vvlocity_namespace__OfferId__r.vlocity_namespace__GlobalKey__c
OverrideDefinition.vlocity_namespace__OverridingAttributeAssignmentId__r.vlocity_namespace__AttributeId__r.vlocity_namespace__Code__c
```

Then, Sync Global Keys of OverrideDefinition.vlocity_namespace__OverridingAttributeAssignmentId__c of source and target

#### Product Child Items  Scenarios:

##### 1. Non Override Product Child Item (IsOverride__c = false )

This will Match Records to Sync the GlobalKeys by using a key generated by the combination of these two Values: 

```
ProductChildItem.vlocity_namespace__ParentProductId__r.vlocity_namespace__GlobalKey__c
ProductChildItem.vlocity_namespace__ChildProductId__r.vlocity_namespace__GlobalKey__c 
```

##### 2. Override Product Child Item  (IsOverride__c = true )

This will query OverideDefinitions by using the considitions:

```
OverrideDefinition.vlocity_namespace__OverrideType__c = 'Attribute' 
OverrideDefinition.vlocity_namespace__OverridingProductChildItemId__c  != null 
OverrideDefinition.vlocity_namespace__OverriddenProductChildItemId__c != null 
```

And Match them across source and Target by the combination of these Values: 

```
OverrideDefinition.vlocity_namespace__ProductHierarchyGlobalKeyPath__c
OverrideDefinition.vlocity_namespace__ProductId__r.vlocity_namespace__GlobalKey__c
OverrideDefinition.vlocity_namespace__PromotionId__r.vlocity_namespace__GlobalKey__c
OverrideDefinition.vlocity_namespace__PromotionItemId__r.vlocity_namespace__ProductId__r.vlocity_namespace__GlobalKey__c
OverrideDefinition.vlocity_namespace__PromotionItemId__r.vlocity_namespace__ContextProductId__r.vlocity_namespace__GlobalKey__c
OverrideDefinition.vlocity_namespace__PromotionItemId__r.vvlocity_namespace__OfferId__r.vlocity_namespace__GlobalKey__c
```

Then, Sync Global Keys of OverrideDefinition.vlocity_namespace__OverridingProductChildItemId__c of source and target

#### How to use a custom unique key for "OverrideDefinition" Match

You can pass as an additional parameter a .yaml file (-d <File Name> ) to use custom unique key for "OverrideDefinition" Match sample File for PCI and AA.

Sample File:

```
PCI:
  - namespace__ProductHierarchyGlobalKeyPath__c
  - namespace__ProductId__r.namespace__GlobalKey__c
  - namespace__PromotionId__r.namespace__GlobalKey__c
  - namespace__PromotionItemId__r.namespace__ProductId__r.namespace__GlobalKey__c
  - namespace__PromotionItemId__r.namespace__ContextProductId__r.namespace__GlobalKey__c
  - namespace__PromotionItemId__r.namespace__OfferId__r.namespace__GlobalKey__c
AA:
  - namespace__ProductHierarchyGlobalKeyPath__c
  - namespace__ProductId__r.namespace__GlobalKey__c
  - namespace__PromotionId__r.namespace__GlobalKey__c
  - namespace__PromotionItemId__r.namespace__ProductId__r.namespace__GlobalKey__c
  - namespace__PromotionItemId__r.namespace__ContextProductId__r.namespace__GlobalKey__c
  - namespace__PromotionItemId__r.namespace__OfferId__r.namespace__GlobalKey__c
  - namespace__OverridingAttributeAssignmentId__r.namespace__AttributeId__r.namespace__Code__c
```


```
USAGE

  $ sfdx vlocityestools:clean:epcgkfix -s <string> -t <string>  [-p cmt] [-c] [-a] [-v]

OPTIONS

  -p, --package=package                                     Vlocity Package Type, Options:
                                                            'cmt' or 'ins' (Optional)

  -s, --source=source                                       Source Org Alias or username

  -t, --target=target                                       Target Org Alias or username

  -c, --pci=pci                                             Match Product Chield Items GK
                                                            (Optional)

  -a, --aa=aa                                               Match Attribuyte Assigments GK
                                                            (Optional)

  -v, --check=check                                         Do not insert Updated Records

  -d, --definitions=definitions                             YAML file Name to Override the 
                                                            unique key for "OverrideDefinition" 
                                                            Match
  

EXAMPLES

  $ sfdx vlocityestools:clean:epcgkfix -s myOrg@example.com -t myOrg2@example.com  -p cmt --pci --aa 
  
  $ sfdx vlocityestools:clean:epcgkfix --source myOrg --target myOrg2 --aa --check

  $ sfdx vlocityestools:clean:epcgkfix --source myOrg --target myOrg2 --aa --check -d def.yaml

```





## vlocityestools:clean:omniscripts

Delete old versions of OmniScritps and leave N amount of latest versions
Active versions will be ignored and wont get deleted.

```
USAGE

  $ sfdx vlocityestools:clean:omniscripts -u <string> -n <integer> -p <string>

OPTIONS

  -u, --targetusername=targetusername                       username or alias for the target
                                                            org; overrides default target org

  -n, --numberversions=numberversions                       Number of most recent versions of
                                                            OmniScrits to keep for each one.
                                                            Has to be greater than 0.

  -p, --package=package                                     Vlocity Package Type, Options:
                                                            'cmt' or 'ins' 

EXAMPLES

  $ sfdx vlocityestools:clean:omniscripts -u myOrg@example.com -n 10 -p cmt
  
  $ sfdx vlocityestools:clean:omniscripts --targetusername myOrg@example.com --numberversions 10 --package ins

```


## vlocityestools:clean:templates

Delete old versions of Templates and leave N amount of latest versions.
Active versions will be ignored and wont get deleted.

```
USAGE

  $ sfdx vlocityestools:clean:templates -u <string> -n <integer> -p <string>

OPTIONS

  -u, --targetusername=targetusername                       username or alias for the target
                                                            org; overrides default target org

  -n, --numberversions=numberversions                       Number of most recent versions of
                                                            Templates to keep for each one.
                                                            Has to be greater than 1.

  -p, --package=package                                     Vlocity Package Type, Options:
                                                            'cmt' or 'ins' 

EXAMPLES

  $ sfdx vlocityestools:clean:templates -u myOrg@example.com -n 5 -p cmt
  
  $ sfdx vlocityestools:clean:templates --targetusername myOrg@example.com --numberversions 5 --package ins

```

## vlocityestools:clean:datapacks

Delete old DataPacks Used by Vlocity Build Tool

```
USAGE

  $ sfdx vlocityestools:clean:datapacks -u <string> -p <string>

OPTIONS

  -u, --targetusername=targetusername                       username or alias for the target
                                                            org; overrides default target org

  -p, --package=package                                     Vlocity Package Type, Options:
                                                            'cmt' or 'ins' 

EXAMPLES

  $ sfdx vlocityestools:clean:datapacks -u myOrg@example.com  -p cmt
  
  $ sfdx vlocityestools:clean:datapacks --targetusername myOrg@example.com  --package ins

```

## vlocityestools:clean:savedomniscripts

Delete old Saved OmniScripts

```
USAGE

  $ sfdx vlocityestools:clean:savedomniscripts -u <string> -p <string>

OPTIONS

  -u, --targetusername=targetusername                       username or alias for the target
                                                            org; overrides default target org

  -p, --package=package                                     Vlocity Package Type, Options:
                                                            'cmt' or 'ins' 

EXAMPLES

  $ sfdx vlocityestools:clean:savedomniscripts -u myOrg@example.com  -p cmt
  
  $ sfdx vlocityestools:clean:savedomniscripts --targetusername myOrg@example.com  --package ins

```

## vlocityestools:compare:folders

Compare two local Vlocity DataPacks folder 
The output will be a CSV file with the results

```
USAGE

  $ sfdx vlocityestools:compare:folders -s <string>- t <integer>

OPTIONS

  -s, --folder1=folder1                                   Vlocity Folder 1 to Compare
                                                

  -t, --folder2=folder2                                   Vlocity Folder 2 to Compare
  


EXAMPLES

  $ sfdx vlocityestools:compare:folders -s vlocity1 -t vlocity2
  
  $ sfdx vlocityestools:compare:folders --folder1 vlocity1 --folder2 vlocity2

```

## vlocityestools:compare:packages

Compare Two Vlocity Metadata Folders. Gives Error if there is overlap

```
USAGE

  $ sfdx vlocityestools:compare:packages -s <string>- t <integer>

OPTIONS

  -s, --folder1=folder1                                   Vlocity Folder 1 to Compare
                                                

  -t, --folder2=folder2                                   Vlocity Folder 2 to Compare
  


EXAMPLES
  
  $ sfdx vlocityestools:compare:packages: -s vlocity1 -t vlocity2

  $ sfdx vlocityestools:compare:packages --folder1 vlocity1 --folder2 vlocity2

```


## vlocityestools:report:dependencies:local


From a local DataPack export foler, for Both OmniScript and VIP, 1st Level of dependencies: 

- DataRaptors
- OmniScripts
- VIPS
- Remote Calls
- VlocityUITemplate

The output will be a CSV file with the results


```
USAGE

  $ sfdx vlocityestools:report:dependencies:local -f <string>

OPTIONS

  -f, --folder=folder                                  Vlocity Folder Name
                                                
  

EXAMPLES

  $ sfdx vlocityestools:report:dependencies:local -f vlocity
  
  $ sfdx vlocityestools:report:dependencies:local --folder vlocity

```

## vlocityestools:report:dependencies:remote

From remote Alias connection, for Both OmniScript and VIP, 1st Level of dependencies: 

- DataRaptors
- OmniScripts
- VIPS
- Remote Calls
- VlocityUITemplate

The output will be a CSV file with the results


```
USAGE

  $ sfdx vlocityestools:report:dependencies:remote -u <string> -p <string>

OPTIONS

  -u, --targetusername=targetusername                       username or alias for the target
                                                            org; overrides default target org

  -p, --package=package                                     Vlocity Package Type, Options:
                                                            'cmt' or 'ins' 
                                                
  

EXAMPLES

  $ sfdx vlocityestools:report:dependencies:remote -u SIT -p cmt
  
  $ sfdx vlocityestools:report:dependencies:remote --targetusername myOrg@example.com  --packageType ins

```

## vlocityestools:report:activeomniscript

Check All OmniScrips are Active

```
USAGE

  $ sfdx vlocityestools:report:activeomniscript -u <string> -p <string>

OPTIONS

  -u, --targetusername=targetusername                       username or alias for the target
                                                            org; overrides default target org

  -p, --package=package                                     Vlocity Package Type, Options:
                                                            'cmt' or 'ins' 
                                                
  

EXAMPLES

  $ sfdx vlocityestools:report:activeomniscript -u myOrg@example.com -p cmt
  
  $ sfdx vlocityestools:report:activeomniscript  --targetusername myOrg@example.com --package ins

```

## vlocityestools:sfsource:createdeltapackage

Based on Vlocity Build Tool saved Hash in the Environment, Create Delta package for salforce.
Note: Only works for SFDX Source Format

--gitcheckkeycustom and --customsettingobject can be used to use a Custom "Custom Settings". For this: Create a new Custom Setting, the API name will be --customsettingobject. This Custom Setting will have two fields "Name" and the value of it will be the "--gitcheckkeycustom" and a field "Value__c" that will contain tha hash.


```
USAGE,

  $ sfdx vlocityestools:sfsource:createdeltapackage -u <string> -d<string> [-k <string>] [-p <string>] [-c <string> -v <string>]

OPTIONS

  -u, --targetusername=targetusername                       username or alias for the target
                                                            org; overrides default target org

  -p, --package=package                                     (Optional) Vlocity Package Type, Options:
                                                            'cmt' or 'ins' 

  -d, --sourcefolder=sourcefolder                           Salesfroce sorce folder name

  -k, --gitcheckkey=gitcheckkey                             (Optional) Key when using gitCheckKey with Build Tool

OPTIONS IF USING a Custom "Custom Settings"

  -c, --customsettingobject=customsettingobject             (Optional) Optional Custom Setting API Object Name 

  -v, --gitcheckkeycustom=gitcheckkeycustom                 (Optional) Custom Setting record Name

  -h, --valuecolumn=valuecolumn                             (Optional) API Field Name Where hash is stored
                                                           
                          

EXAMPLES

  $ sfdx vlocityestools:sfsource:createdeltapackage -u myOrg@example.com -p cmt -d force-app
  
  $ sfdx vlocityestools:sfsource:createdeltapackage --targetusername myOrg@example.com --package ins --sourcefolder force-app

  $ sfdx vlocityestools:sfsource:createdeltapackage --targetusername myOrg@example.com --package ins --sourcefolder force-app --gitcheckkey EPC

  $ sfdx vlocityestools:sfsource:createdeltapackage --targetusername myOrg@example.com --sourcefolder force-app --gitcheckkeykustom VBTDeployKey --customsettingobject DevOpsSettings__c

```

## vlocityestools:sfsource:createdeltapackagelocal

Create Delta package for salforce based on HEAD and Input Hash.
Note: Only works for SFDX Source Format

```
USAGE,

  $ sfdx vlocityestools:sfsource:createdeltapackagelocal -h <string> -d<string> 

OPTIONS

  -h, --hash=hash                                           Git Hash Value 

  -d, --sourcefolder=sourcefolder                           Salesfroce sorce folder name

                          

EXAMPLES

  $ sfdx vlocityestools:sfsource:createdeltapackagelocal -h f2a6eee1b509c3edd33ab070148be48e41242846 -d force-app
  
  $ sfdx vlocityestools:sfsource:createdeltapackagelocal --hash f2a6eee1b509c3edd33ab070148be48e41242846 --sourcefolder salesforce_sfdx

```

## vlocityestools:sfsource:updatedeltahash

When using a Custom "Custom Setting" Object for delta Package. You can update the hash in the environment using this command.

```
USAGE,

  $ sfdx vlocityestools:sfsource:createdeltapackage -u <string> -p <string> -d<string> [-h <string>]

OPTIONS

  -u, --targetusername=targetusername                       username or alias for the target
                                                            org; overrides default target org

  -c, --customsettingobject=customsettingobject             Custom Setting Object API Name 

  -v, --gitcheckkeycustom=gitcheckkeycustom                 Custom Setting record Name

  -h, --customhash=customhash                               (Optional) Custom Hash Value to be updated

                                                           
                          

EXAMPLES

  $ sfdx vlocityestools:sfsource:updatedeltahash  -c DevOpsSettings__c -v DeployKey -u myOrg@example.com

  $ sfdx vlocityestools:sfsource:updatedeltahash  --customsettingobject DevOpsSettings__c --gitcheckkeycustom DeployKey --targetusername myOrg@example.com
   
  $ sfdx vlocityestools:sfsource:updatedeltahash  --customsettingobject DevOpsSettings__c --gitcheckkeycustom DeployKey --targetusername myOrg@example.com -- 0603ab92ff7cf9adf7ca10228807f6bb6b57a894

```

## vlocityestools:clean:calcmatrix

This command will delete all rows of the Calculation Matrix Version based on the given ID as input. Then, it will update the version record with Dummy data so any other version can be Deployed. The Calculation Matrix Version can be delete after 24 hors due to Salesforce sweeper restrictions.
You can assign the user used to run this comnad a Permisison set or a profile that has the Bulk API hard delte to avoid the need of deleting the rows from the recycle bin.

```
USAGE

  $ sfdx vlocityestools:clean:calcmatrix -u <string> -i <string> -P<string>

OPTIONS

  -u, --targetusername=targetusername                       username or alias for the target
                                                            org; overrides default target org

  -i, --matrixid=matrixid                                   Matrix Version ID to be clean 


  -p, --package=package                                     Vlocity Package Type, Options:
                                                            'cmt' or 'ins' 
                                                           
                          

EXAMPLES

  $ sfdx vlocityestools:clean:calcmatrix -u myOrg@example.com -i a0dR000000kxD4qIAE -p ins
  
  $ sfdx vlocityestools:clean:calcmatrix --targetusername myOrg@example.com --matrixid a0dR000000kxD4qIAE --package cmt

```

## vlocityestools:auth:login

Create an Alias using User and Password,  (Token if needed as wells)
Note: This will not create an Alias with OAuth Connnection so the connection will expired.

```
USAGE

  $ sfdx vlocityestools:auth:login -u <string> -p <string> -a <string> [-l <string>] [-t <string>]

OPTIONS

  -u, --username=username                                   Username to Autenticate

  -p, --password=password                                   Password

  -t, --token=token                                         Token
  
  -l, --url=url                                             Org Url, Default: https://login.salesforce.com

  -a, --alias=alias                                         Alias


                                                                    

EXAMPLES

  $ sfdx vlocityestools:auth:login -u jgonzalez@vlocity.com.de1 -p 'pass123' -t eXUTfa9gpIxfaytnONqnlWFG -a dev1
  
  $ sfdx vlocityestools:auth:login --username jgonzalez@vlocity.com.de1 --password 'pass123' --url 'https://test.salesforce.com' --alias dev1

```


## vlocityestools:clean:objects

Delets SObjects from org defined by File. SOQL 'WHERE' can be specified

Sample File:

![Org to Org Comparison](images/SampleObjectsFile.png)

```
USAGE

  $ sfdx vlocityestools:clean:objects -u <string> -p <string> -d <string> [-q true|false] [-r true|false] [-s true|false]

OPTIONS

  -u, --username=username                                   Username to Autenticate

  -p, --package=package                                     Vlocity Package Type, Options:
                                                            'cmt' or 'ins' (Optional)

  -d, --datafile=datafile                                   File with list Of Objects
  
  -q, --onlyquery=onlyquery                                 Dont Delete Any Object, just do queries.
                                                            Default: false

  -r, --retry=retry                                         Retry all Deletes if Error
                                                            Default: false

  -s, --save=save                                           Save Batch Results in File
                                                            Default: false
  -h, --hard=hard                                           Hard delete all records
                                                            This needs the System permission:
                                                            "Bulk API Hard Delete"

  -t, --polltimeout=polltimeout                              Bulk API Poll time out in Minutes (Default 60)
                                                                    
EXAMPLES

  $ sfdx vlocityestools:clean:objects -u myOrg@example.com -p ins -d objects.yaml
  
  $ sfdx vlocityestools:clean:objects --targetusername myOrg@example.com --dataFile objects.yaml

  $ sfdx vlocityestools:clean:objects --targetusername myOrg@example.com --dataFile objects.yaml -q true

  $ sfdx vlocityestools:clean:objects --targetusername myOrg@example.com --dataFile objects.yaml -r true

  $ sfdx vlocityestools:clean:objects --targetusername myOrg@example.com --dataFile objects.yaml -q true

```


## vlocityestools:sfsource:createmocklwcos

Deploy Empty LWC for Missing OmniScript

```
USAGE

  $ sfdx vlocityestools:sfsource:createmocklwcoss -d <string> 

OPTIONS

  -u, --username=username                                   Username to Autenticate

  -d, --datapacksfolder=datapacksfolder                     DataPacks folder
                                                                    

EXAMPLES

  $ sfdx vlocityestools:auth:login -u jgonzalez@vlocity.com.de1 -p 'pass123' -t eXUTfa9gpIxfaytnONqnlWFG -a dev1
  
  $ sfdx vlocityestools:auth:login --username jgonzalez@vlocity.com.de1 --password 'pass123' --url 'https://test.salesforce.com' --alias dev1

```
