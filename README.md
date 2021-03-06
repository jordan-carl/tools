# Overview

The purpose of the emote tool is to create projects that contain Emotive applications and deploy them to the Emotive
cloud service. The emote project consists of the files that reside on your local file system. The Emotive cloud service
does not keep track of your project. When you deploy a project, the contents the the project, such as apps and
integration proxies, are uploaded to the Emotive cloud service.

## Installing emote

emote is implemented in node.js, which must be installed prior to installing emote. See [here](http://nodejs.org/download/)
for installation instructions. emote requires node.js v0.8.3 or higher.

IMPORTANT: before proceeding to install emote, run this simple test to verify your node.js and node package manager install:

    $ npm install request 

This installs the "request" package, and it insures that npm is configured correctly. Don't skip this step
unless you are already using node in your development system. [If the very first install using npm uses the '-g' option (see below) it triggers a bug in npm that will make later installs difficult.]

emote is then installed using npm, the node package manager. If you install with the '-g' option, emote will be in your path and available from the command line.

On a PC:

    $ npm install -g emote

On a OS-X or Linux system:

    $ sudo npm install -g emote

## Example project

Here is an example of creating a project. The commands used are described in more detail below.

    $ emote create myproj
    Created empty project myproj
    $ cd myproj
    $ ls
    app     model       project.json    proxy
    $ 

emote creates a project directory that contain a project.json file and a subdirectory for app, model, and proxy code.

Project specific settings are stored in project.json. This file is also used to identify the root of a project. If you
are in a project subdirectory and issue an emote command it finds the root of the project by going up the directory
structure until project.json is found.

### Listing available a sample applications

There are a number of sample applications available from the emotive mms. Use the "list" command to see their names:

    $ emote list sample -g
    Connecting to MMS as user mark@emotive.com at https://test.mms.emotive.com

    targetType   name                     lastModifiedAt
    -----------  -----------------------  --------------------  
    CdmResource  sample-AppList           2013-01-30T20:43:35Z
    CdmResource  sample-AppListjQueryUI   2013-01-30T20:43:35Z
    CdmResource  sample-AppListNeutral    2013-01-30T20:43:35Z
    CdmResource  sample-AppListTablet     2013-01-30T20:43:36Z
    CdmResource  sample-BarCode           2013-01-30T20:43:36Z
    CdmResource  sample-Camera            2013-01-30T20:43:36Z
    CdmResource  sample-GoogleMaps        2013-01-30T20:43:36Z
    CdmResource  sample-GPS               2013-01-30T20:43:36Z
    CdmResource  sample-HelloWorld        2013-01-30T20:43:36Z
    CdmResource  sample-Messaging         2013-01-30T20:43:36Z
    CdmResource  sample-Pagination        2013-01-30T20:43:36Z
    CdmResource  sample-PerfTest          2013-01-30T20:43:37Z
    CdmResource  sample-SalesforceLeads   2013-01-30T20:43:37Z
    CdmResource  sample-SignatureCapture  2013-01-30T20:43:37Z
    CdmResource  sample-SplitView         2013-01-30T20:43:37Z
    CdmResource  sample-USGS              2013-01-30T20:43:37Z
    CdmResource  sample-Weblet            2013-01-30T20:43:37Z
    CdmResource  sample-WhereAmI          2013-01-30T20:43:37Z
    18 rows.
    $ 

### Downloading a sample application into your project

Use the download command to bring a copy of a sample application into your project:

    $ emote download sample sample-USGS
    Connecting to MMS as user mark@emotive.com at https://test.mms.emotive.com
    Finished downloading sample: sample-USGS
    $ ls */USGS
    app/USGS:
    icon114.png     icon48.png      icon72.png      source.js
    icon144.png     icon57.png      source.html     source.properties

    model/USGS:
    model.json

    proxy/USGS:
    README.md   index.js    package.json
    $ 

The download commands copies an app, model, and proxy for the USGS Earthquake Package into your project.

### Credentials and the profile.json file

After creating your project, additional emote commands, like list, often require communicating with the Emotive cloud service (MMS). This requires credentials, including the username and password you were provided when signing up to use the Emotive cloud service. The default profile.json created in your project causes emote to prompt for server, username, and password:

    $ cat profile.json 
    {
      "server": "<<Emotive server URL>>https://dev.emotive.com",
      "username": "<<Emotive username>>",
      "password": "<<Emotive password>>"
    }$ 

The << >> notation indicate that the system should prompt. Any value after the << >> is a default value. You can replace the values in this file with your credentials if you want to avoid typing them on each invocation of emote. E.g.

    $ cat profile.json 
    {
      "server": "https://dev.emotive.com",
      "username": "me@mydomain.net",
      "password": "<<Emotive password>>"
    }$ 

With this profile.json, it will only prompt for the password, which saves some typing. You could also put your cleartext password in the profile.json if you plan on keeping the file in a secure location.

### Deploying your project to the emotive MMS

The deploy command uploads your app, model, and proxy code to MMS.

    $ emote deploy
    Connecting to MMS as user mark@emotive.com at https://dev.emotive.com
    npm http GET https://registry.npmjs.org/emproxy
    npm http GET https://registry.npmjs.org/request
    npm http 200 https://registry.npmjs.org/emproxy
    npm http 200 https://registry.npmjs.org/request
    npm http GET https://registry.npmjs.org/emutils
    npm http GET https://registry.npmjs.org/portfinder/0.2.1
    npm http 200 https://registry.npmjs.org/portfinder/0.2.1
    npm http GET https://registry.npmjs.org/portfinder/-/portfinder-0.2.1.tgz
    npm http 200 https://registry.npmjs.org/emutils
    npm http GET https://registry.npmjs.org/emutils/-/emutils-1.0.8.tgz
    npm http 200 https://registry.npmjs.org/portfinder/-/portfinder-0.2.1.tgz
    npm http 200 https://registry.npmjs.org/emutils/-/emutils-1.0.8.tgz
    npm http GET https://registry.npmjs.org/mkdirp
    npm http 200 https://registry.npmjs.org/mkdirp
    USGS@1.0.0 node_modules/USGS
    ├── request@2.9.203
    └── emproxy@1.0.9 (emutils@1.0.8, portfinder@0.2.1)
    Exec'ing query file: model.json
    CREATED CdmExternalSystem: USGS
    Inserted CdmTypeBinding: Feature
    Completed binding of CDM type: Feature
    Finished executing directives.
    Deployed model USGS
    Uploading resource: USGS of length 122632
    Deployed proxy USGS
    Uploading app: USGS of length 65418
    App USGS added
    Deployed app USGS
    $ 

Note from the example output that deploy has several intermediate steps. First it does a "build" to make sure that the proxies have been installed using npm. The npm install will cause any dependencies to be pull into the staging area. After that, it deploys the model, the proxy, and the app for the USGS sample by uploading them to http://dev.emotive.com.

After running emote deploy, you will be able to run the app and view the USGS earthquake data on your mobile device (i.e. iPhone, Android tablet, etc.)

### Adding modules

The "add" command adds additional content to your project. "modules" are created under the app, model, and/or proxy directories. When a "template" is specified for the module being added, emote copies initial code into the module directory. For example, the following command adds a proxy to "myproj":

    $ emote add proxy myprox1 --template Salesforce
    Adding proxy from template Salesforce
    $ ls proxy
    myprox1
    $ ls proxy/myprox1
    README.md   index.js    package.json
    $ 

In this example, a new proxy, "myprox1" has been added the used the Salesforce template. A proxy that allows easy integration of your app with Salesforce has been included. The Saleforce proxy is a Node.js module. The package.json file contains the parameters needed to deploy this proxy using npm.

In an emote project, modules work to provide the functionality needed by your emotive application. The module name for a proxy and for a model correspond to the "externalSystem" name of your emotive application. In the next example, we add a "model" module to work with our proxy:

    $ emote add model myprox1 --template Salesforce
    Adding model from template Salesforce
    $ ls model/
    myprox1
    $ ls model/myprox1/
    salesforce.json
    $ 

The Salesforce template for the model creates a "salesforce.json" file that contains metadata that specifies what Salesforce object you would like to integrate into your appication, and how they correspond with the emotive CDM (common data model.)

# Commands

## Common Parameters

## module types

Several commands include a module type (or group of module types) as
one of their parameters.  Module types include:

* app
* model
* operation 
* proxy 
* test
* theme

A module type is a directory that is a child of the Emote project directory.

A module type parameter can be:

1. A single type e.g. __app__ or __model__
2. A group of module types joined by __+__, e.g. __app+theme__ or
__proxy+test+operation__
3. __all__, meaning all module types

## module names

A module name is the name of a directory that is a child of a module type directory. For models and proxies, this will be the same as the "externalSystem" name in the CDM.

## Commands

### emote default action

With no parameters or options, emote provide basic information on the context in which it is running. E.g.

    $ emote
    version   2.2.0
    profile   /Users/mark/profile.json
    username  mark@emotive.com on https://dev.emotive.com
    project   myproj
    $ 

The following options can only be used when emote is passed no other parameters:

* --help : show a summary of the available commands and options
* --info : default, the same as 'emote' with no options, as above.
* --readme : outputs this README.md file to the console
* --emoteDir : prints the directory in which the emote source code is deployed
* --version : prints the version on emote
* --whoami : prints the current username for emote
* --doc : prints the URL to doc.emotive.com

### add

`emote add [`_moduleType_ `[`_moduleName_`]] [--template` _templateName_]

_moduleType_ is a standard module type parameter (see above). 

_moduleName_ is a standard module name parameter.

__--template__ is optional. If it is included, the specifid parts from the named template (e.g. Salesforce) will be included in the project using the specified module name (which defaults to __default__).

If __--template__ is omitted, _moduleType_ and _moduleName_ are required, and an empty directory _ProjectRoot/moduleType/moduleName_ is created.

### create

`emote create` _projectName_

Create a new Emotive project in the current directory. _projectName_ is the name of the directory created. Subdirectories __model__, __app__, __proxy__ are created. 

profile.json is created with default values that cause emote to prompt for credentials. (See profile.json above.)

### download

`emote download` (app | sample) _identifier_

This command will download a previously deployed artifact from the MMS cloud service and save as files in your project
directory. Currently, this is only implemented for apps and samples. 

For apps, the command is:

`emote download app` _appId_

Example:

    emote download app usgs

will download the app with ID "usgs" from your tentant and add it to your current project. Note that your current project is found using on your current directory.

For samples, the command is:

`emote download sample` _sampleName_

Where the list of sample names can be obtained by using:

    emote list sample -g


### deploy

`emote deploy [`_moduleType_ `[` _moduleName_ `]]`

_moduleType_ is a standard module type parameter.

_moduleName_ is a standard module name parameter.

The __deploy__ command works within any subdirectory of a project. With no parameters, __deploy__ will deploy the entire project.  If parmeters are present, they specify which parts of the project to deploy.

Example:

    emote deploy proxy usgs

Will deploy the "usgs" proxy that was generated into your project.

Note that until a project has been deployed, the MMS cloud service (at mms.emotive.com) has no knowledge of the project.
By deploying the generated project, the app is made available to users.

### undeploy

`emote deploy [`_moduleType_ `[` _moduleName_ `]]`

_moduleType_ is a standard module type parameter.

_moduleName_ is a standard module name parameter.

This removes objects that have been deployed. E.g.

    emote undeploy app usgs

Removes the app named __usgs__.

Undeploy is currently implemented only for app, model, and theme.

Undeploying a model, e.g.

    emote undeploy model

removes the CdmExternalSystem definitions for the tenant and all bindings to those external systems. It does not remove any CdmType's that were part of the model, but those types will no longer be bound to an external system.

    emote undeploy theme custom

This will undeploy the theme named __custom__ (the theme name defaults to "default" if none is specified).

### build ###

`emote build  [`_moduleType_ `[` _moduleName_ `]]`

This does whatever is necessary to build the project before deploying it.  By default, it builds the entire project.  If _moduleType_ and/or  _moduleName_ are specified, only that part of the project is built.

It is not necessary to buiuld a project before deploying it; the __deploy__ comand builds the project automatically.

### list

`emote list` _artifactType_ _artifactName_ `[-g]`

The __-g__ option indicates that "global" objects, those that were created by the system rahter than the current tenant, should also be listed.

_artifactType_ is one of:

* app :  list deployed applications, 
* model : list defined CdmType's 
* proxy : list the resource for a deployed proxy
* sample : list the resource for deployed sample projects
* resource : list all resources
* template : list template available in emote (see 'add' command)
* externalSystem : list CdmExternalSystem objects

_artifactName_ can be wildcarded using leading, trailing, or embedded '*' characters. When wildcards are used, the artifact name should be quoted. E.g.

    emote list app 'A*'

Will list all the apps where the name starts with 'A'.

    emote list resource '*cdf*' -g

Lists all the resources that contain 'cdf' in their name. Global resources included. 
 

### log

`emote log` _startTime_ _endTime_ `[--tail] [--local] [--csv]`

This displays the log file for your proxy running on the MMS server. __startTime__ indicates the stamp of the earliest message to show. It can be specified in several formats.

* GMT specified as ISO 8601: `2012-09-13T17:30:00`
* Local time specified as ISO 8601 and including the offset to the timezone: `2012-09-13T10:30:00-07:00`
* Local time, assuming your current timezone (the quotes are required): `"2012-09-13 10:30:00"`
* Relative time (as in, "starting 5 minutes ago"): `5m`
* Relative time (as in, "starting 2 hours ago"): `2h`

If you specify just __startTime__ you will get all the log entries that have been recorded since that time until the current moment. If you specify __endTime__ as well, you will only see those records between __startTime__ and __endTime__.

You can add the __--tail__ option which will show all the records from the__startTime__ to the present, and then
periodically append new log records as they arrive. (If you specify __--tail__ with no __startTime__ it will assume you wanted a __startTime__ of __5m__.)

The output is normally shown with GMT timestamps, but if you add the __--local__ option the timestamps will be converted
to your local time zone (and the times will have an "L" appended to remind you it's not GMT).

The output is normally designed to be human-readable, but you can specify the __--csv__ option and the output will be
written in CSV format so it could be fed into another program.

Some common examples:

Show the log data for the previous 5 minutes then tail the log to show new output as it arrives:

    emote log --tail

Show the log data for the previous 2 minutes then tail the log to show new output as it arrives, converting the timestamps to local time:

    emote log 2m --tail --local

Show the log data for a certain date from 9 to 10 am in the local time zone, output in CSV format:

    emote log "2012-09-13 09:00:00" "2012-09-13 10:00:00" --csv


### exec

`emote exec` _commandFile_`.json`

This takes a parameter which is the name of a file containing JSON for an Array of MMS REST requests. It submits the
requests to MMS as a synchronously run series of commands, and logs the REST responses to stdout.

Example:

    emote exec myfile.json

Where myfile.json contains:

    {"op":"SELECT", "targetType":"CdmExternalSystem"}

Will select a list of the External Systems for your tenant which are registered with MMS and print it to stdout.

### getWsdl

`emote getWsdl` _moduleName_ _wsdlUrl_ _service_ _port_ [_username_ _password_]

This creates several files in several different module types, all in the module _moduleName_.

_moduleName_ is a standard module name.

_wsdlUrl_, _service_, _port_, specifies a WSDL port which the module will comminicate with.  The service and port are specified by their unqualified names. 

This is the first step in creating a SOAP-service-based subproject.  It will fetch the WSDL for a web service and create three
files in your project's <subproject>/model directory:

* wsdl.json contains a JSON version of the web service's definitions.
* wsdlOps.json contains a list of the web service's operations.
* proxyConfig.json allows you to configure the connection from the generated proxy to toe target SOAP service.

wsdlOps.json determined which web service operations will be part of the generated subproject.  Originally, all of the
operations are disabled (set to false.) Before generating, edit this file to set the desired operations to "true".

proxyConfig.json will optionally pass these connection properties to the generated proxy. (By default, all are set to null,
so the default behavior applies.)

* By default, the proxy will expect to find the SOAP service at the soapAddress given in the WSDL port.  If this is
  incorrect, set the soapAddress property in proxyConfig.json to the correct service endpoint URL.
* By default, no HTTP[S] authentication is used when talking to the SOAP service.  If basic authentication is required,
  set the user name and password in proxyConfig.json.

After editing these two files, use the generateFromWsdl command to generate your subproject.

### generateFromWsdl

`emote generateFromWsdl` _moduleName_

This will generate a SOAP-service-based subproject.  Before running this, you must use the getWsdl command to load WSDL-based definitions into your module.

### status

`emote status`

Check the availability of the Emote server.

### test

`emote test` [_moduleName_]

Run the Javascript tests in the test directory of the current project.  If _moduleName_ is specified, run only the test in that subdirectory.

# Credentials

Here are the methods of supplying MMS credential to emote.

## .emote\_profile

MMS credentials can be using a ".emote\_profile" file, which can be located in the user directory. This is a JSON file. Example:

    {
        "username":"myuser",
        "password":"mypassword",
        "server":"https://mms.emotive.com"
    }

## profile.json in project

If the file "profile.json" is present in the root of a project, it is used for commands within that project. It has
the same format as .emote\_profile

## Profile file supplied on command line

The profile file name can also be included as a command line argument:

    emote --profile alt\_profile.json <command> ...

## Credentials on command line

A username and password (and optionally a server URL) can be supplied on the command line:

  --password, -p  Emotive password                               
  --username, -u  Emotive username                               
  --server, -s    Emotive server URL [default: "http://mms.emotive.com"]

## Precedence

Starting with the lowest precedence, each source of credential described above overrides the ones before it. So the precedence is:

1. Username and password as command line parameters
2. Profile file on command line
3. "profile.json" in project
4. .emote\_profile

Individual values are overridden, so "username" may be taken from one profile, while "server" is defined in another. Properties in "externalCredentials" are merged by name.
