# Salesforce to Database and a Webservice Demo

+ [License Agreement](#licenseagreement)
+ [Use Case](#usecase)
+ [Considerations](#considerations)
	* [Salesforce Considerations](#salesforceconsiderations)
+ [Run it!](#runit)
	* [Running on premise](#runonopremise)
	* [Running on Studio](#runonstudio)
	* [Running on Mule ESB stand alone](#runonmuleesbstandalone)
	* [Running on CloudHub](#runoncloudhub)
	* [Deploying your Anypoint Template on CloudHub](#deployingyouranypointtemplateoncloudhub)
	* [Properties to be configured (With examples)](#propertiestobeconfigured)
+ [API Calls](#apicalls)
+ [Customize It!](#customizeit)
	* [config.xml](#configxml)
	* [businessLogic.xml](#businesslogicxml)
	* [endpoints.xml](#endpointsxml)
	* [errorHandling.xml](#errorhandlingxml)


# License Agreement <a name="licenseagreement"/>
Note that using this example is subject to the conditions of this [License Agreement](AnypointTemplateLicense.pdf).
Please review the terms of the license before downloading and using this example. In short, you are allowed to use the template for free with Mule ESB Enterprise Edition, CloudHub, or as a trial in Anypoint Studio.

# Use Case <a name="usecase"/>
Pickup closed opportunities in SFDC and insert them into a database and call a webservice. This use case also inllustrates how the main application mss can be broken up into two applications where one of them are exposing an api that is consumed by the other application. The application mss is an example and the second two mss-order-api-sol and mss-order-consumer-sol are solution projects that would give the final implementation. However the user is encouraraged to build this out on his/her own.

# Run it! <a name="runit"/>
Simple steps to get this example running


### Where to Download Mule Studio
First thing to know if you are a newcomer to Mule is where to get the tools.

+ You can download Mule Studio from this [Location](http://www.mulesoft.com/platform/mule-studio)


### Create a Salesforce Dev account

+ You can sign up for a sfdc dev account here [Location](https://developer.salesforce.com/signup)

### Importing an Example into Studio
This git repo contains 3 different Anypoint Studio projects mss, mss-order-api-sol and mss-order-consumer-sol. All of these have to be imported into studio idividually. Start by importing the mss project which is the starting point for this example. The other two mss-order-api-sol and mss-order-consumer-sol are more solutions for the excercise part of this.
Mule Studio offers several ways to import a project into the workspace, for instance: 

+ Anypoint Studio generated Deployable Archive (.zip)
+ Anypoint Studio Project from External Location
+ Maven-based Mule Project from pom.xml
+ Mule ESB Configuration XML from External Location

You can find a detailed description on how to do so in this [Documentation Page](http://www.mulesoft.org/documentation/display/current/Importing+and+Exporting+in+Studio).


### Running on Studio <a name="runonstudio"/>
Once you have imported example into Anypoint Studio you need to follow these steps to run it:

+ Locate the properties file `mule-project.xml`, in the project root
+ Complete all the properties required
+ Once that is done, right click on you Anypoint Example project folder 
+ Hover you mouse over `"Run as Mule Application with Maven"`

After this you need to trigger the process from Salesforce:
+ Create a new opportunity
+ Fill in mandatory data and + account
+ Add a product and a quantity
+ Update the status to closed won.
+ Verify in the studio logs that the opp has been detected during the next sync cycle.
+ Verify that the description field in SFDC has been updated.



## Deploy to CloudHub <a name="runoncloudhub"/>
While [creating your application on CloudHub](http://www.mulesoft.org/documentation/display/current/Hello+World+on+CloudHub) (Or you can do it later as a next step), you need to go to Deployment > Advanced to set all environment variables detailed in **Properties to be configured** as well as the **mule.env**.


### Deploying your Anypoint Example on CloudHub <a name="deployingyouranypointtemplateoncloudhub"/>
Mule Studio provides you with really easy way to deploy your Example directly to CloudHub, for the specific steps to do so please check this [link](http://www.mulesoft.org/documentation/display/current/Deploying+Mule+Applications#DeployingMuleApplications-DeploytoCloudHub)


## Properties to be configured (With examples) <a name="propertiestobeconfigured"/>
In order to use this Mule Anypoint Template you need to configure properties (Credentials, configurations, etc.) either in properties file or in CloudHub as Environment Variables. Detail list with examples:

#### SalesForce Connector configuration for company A
+ sfdc.user`jorge.drexler@mail.com`
+ sfdc.password `Noctiluca123`
+ sfdc.token `avsfwCUl7apQs56Xq2AKi3X`
+ sfdc.system.user= `A0ed000BO9T`  



# API Calls <a name="apicalls"/>
Salesforce imposes limits on the number of API Calls that can be made. Therefore calculating this amount may be an important factor to consider. The Anypoint Template calls to the API can be calculated using the formula:

***1 + X + X / 200***

Being ***X*** the number of Users to be synchronized on each run. 

The division by ***200*** is because, by default, Users are gathered in groups of 200 for each Upsert API Call in the commit step. Also consider that this calls are executed repeatedly every polling cycle.	

For instance if 10 records are fetched from origin instance, then 12 api calls will be made (1 + 10 + 1).


# Customize It!<a name="customizeit"/>
This brief guide intends to give a high level idea of how this Anypoint Template is built and how you can change it according to your needs.
As mule applications are based on XML files, this page will be organized by describing all the XML that conform the Anypoint Template.
Of course more files will be found such as Test Classes and [Mule Application Files](http://www.mulesoft.org/documentation/display/current/Application+Format), but to keep it simple we will focus on the XMLs.

Here is a list of the main XML files you'll find in this application:

* [mss.xml](#configxml)
* [mock-soap.xml](#mocksoap)


## config.xml<a name="configxml"/>
Configuration for Connectors and [Properties Place Holders](http://www.mulesoft.org/documentation/display/current/Configuring+Properties) are set in this file. **Even you can change the configuration here, all parameters that can be modified here are in properties file, and this is the recommended place to do it so.** Of course if you want to do core changes to the logic you will probably need to modify this file.

In the visual editor they can be found on the *Global Element* tab.


## mock-soap.xml<a name="mocksoap"/>
This file is packaged in order to provide a mocked soap service.
