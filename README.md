Vlocity ES Tools (Beta)
==============

##### Disclaimer: This tool is not an offical tool from Vlocity. It was created with the intent of distributing certain uitlility tools, Use it at your own risk.


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

### vlocityestools:clean:omniscripts
### vlocityestools:compare:folders
### vlocityestools:clean:datapacks
### vlocityestools:clean:savedomniscripts
### vlocityestools:report:dependencies:local
### vlocityestools:report:dependencies:remote
  
'    '

# Commands Info:

## vlocityestools:clean:omniscrtips

Delete old verions of OmniScritps and leave X amount of latest verions

```
USAGE

  $ sfdx vlocityestools:clean:omniscripts -u <string> -v <integer> -p <string>

OPTIONS

  -u, --targetusername=targetusername                       username or alias for the target
                                                            org; overrides default target org

  -n, --numberversions=numberversions                       Number of most recent versions of
                                                            OmniScrits to keep for each one.
                                                            Has to be greater than 0.

  -p, --package=package                                     Vlocity Package Type, Options:
                                                            'cmt' or 'ins' 

EXAMPLES

  $ sfdx vlocityestools:clean:omniscripts -u myOrg@example.com -n 5 -p cmt
  
  $ sfdx vlocityestools:clean:omniscripts --targetusername myOrg@example.com --numberversions 5 --package ins

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

Compare two local Vlocity Metadata folder 
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


## vlocityestools:sfsource:createdeltapackage

Based on Vlocity Build Tool saved Hash in the Environment, Create Delta package for salforce.
Note: Only works for SFDX Source Format

```
USAGE

  $ sfdx vlocityestools:sfsource:createdeltapackage -u <string> -p <string> -d<string>

OPTIONS

  -u, --targetusername=targetusername                       username or alias for the target
                                                            org; overrides default target org

  -p, --package=package                                     Vlocity Package Type, Options:
                                                            'cmt' or 'ins' 

  -d, --sourcefolder=sourcefolder                           Salesfroce sorce folder name
                                                           
                          

EXAMPLES

  $ sfdx vlocityestools:sfsource:createdeltapackage -u myOrg@example.com -p cmt -d force-app
  
  $ sfdx vlocityestools:sfsource:createdeltapackage --targetusername myOrg@example.com --package ins --sourcefolder force-app

```
