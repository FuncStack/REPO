# Module and Platform Spec

###### Version 1.0

## Summary
 
To package, version and distribute the Infrastructure-as-Code templates, FuncStack proposes the Module format.
Each module is a group of infrastructure resources that support certain infrastructure functionality, such as compute, database and etc.

FuncStack Platform is an higher-level abstracted entity that could be used to run certain type of applications. A platform contains at least one compute module.
The platform could be provisioned and installed with fs-cli or funcstack.com. The provisioned platform is called **Environment**

## Module Spec

### Naming Convention
To publish the module to the FuncStack public module repo, the module naming is following:  

`
{ModuleType}-{Provider}-{CustomName}
`
Supported Module types: compute, database, service

### Module File Structure

```
compute-aws-myvmmodule
│
│   modulespec.yml
|
└───templates
│   │   root.yml
│   │   ec2.yml
│   │   network.yml
│   │
│   └───subfolder1
│       │   file111.yml
│       │   file112.yml
│       │   ...
│   
└───values
    │   values-default.yml
```

### Module Spec File
```
name: compute-aws-myvmmodule
version: 0.0.1
provider: aws
```
Specifies the name, version and provider.   The version follows SemVer2.0 format.  Providers currently includes: aws, gcp and azure.

### templates/root.yml

root.yml is the entrance and main template for the module. All the other templates should be imported and nested in root.yml.

## Platform Spec

### Naming Convention
To publish the platform to the FuncStack public platform repo, the platform naming is following:  
`
{Provider}-{AppType}-{CustomName}
`
Supported App types: web, serverless, cluster

### Platform File Structure
```
aws-web
│
│   platformspec.yml
|
└───dist
|   │   compute-aws-vm-0.0.1.zip
|
└───modules
│   │
│   └───local-module
│       │   modulespec.yml
│       │   ...
│   
└───values
    │   values-default.yml
    │   values-{envName}.yml
```

#### modules
Put the local modules into this folder. 

#### values

### Platform Spec File
```
name: aws-web
version: 0.0.1
provider: aws
modules:
  mycompute:
    type:  ./modules/local-example
  mycompute2:
      type:  compute-aws-vm-0.0.1  
```
A platform could include one or many modules and must have at least one compute type module. If it is not the local module, the public repo will be searched based on the name and version.


