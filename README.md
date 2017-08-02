# My Organization API

This is an API exposing resources from MyOrganization.
In particular Customer information can be retrieved, created, updated and deleted.
The API definition is within the src/main/api/api.raml file, and makes use of Mule APIKit for implementation.

The API implementation with Mule, can be started via Mule Anypoint Studio.
The following instructions provide a guide on how to import, set-up, test and execute this API locally.

## Getting Started

You will need to clone this git repository, as a Mule Project within the Anypoint Studio.
Once imported successfully, you will utilise maven commands to compile and package the API on your local machine.


The project consists of the following core files for your reference:
1. pom.xml - Maven build file
2. /src/main/api/api.raml - RAML file containing the master definition for this API
3. /src/main/app/api.xml - Main Mule XML containing API flows and associated (stubbed) sub-flows.
3. /src/test/munit/myorganization-api-test.xml - MUnit XML for core functions and resources defined for this API
4. /src/test/munit/myorganization-subflow-test.xml - Munit XML for stubbed subflows utilised in this API.
5. /src/test/resources/myorganizationapi/samples/customer - Munit sample input and output files for the Customer resource.

### Prerequisites

In order to have this project executed successully, you will need to have the following installed:
1. Mule Anypoint Studio v6.2.2 or later with Mule 3.8.1 Runtime
2. Maven 3.3.9 or later
3. Java JDK 1.8.0_66 or later
4. Postman to execute various API functions

### Installing

**Adding Java JDK within Anypoint Studio (if not done so already)**
1. Once Anypoint Studio is open, go to Window > Preferences > Java > Installed JREs.
2. Click 'Add...' > Standard VM > 'Directory' next to JRE home and navigate to the Java JDK 1.8.0_66 folder.
3. Once loaded, click finish and the JDK should now be set-up. Ensure that this is the JDK runtime environment configure.

**Adding Maven within Anypoint Studio (if not done so already)**
1. Once Anypoint Studio is open, go to Window > Preferences > Anypoint Studio > Maven
2. In the Maven installation home directory field browse to the Maven Installation directory.
3. In the 'Base command line for builds' enter **_mvn clean test_**. This will be the maven command executed when the project is run for execution of the unit tests.

**Importing the MyOrganization API Mule Maven Project**
1. Once Anypoint Studio is open, Right-Click within the Package Explorer, and select 'Import...'.
2. Expand the 'Anypoint Studio' folder, and select 'Maven-based Mule Project from pom.xml'
3. Select the '...' button next to the 'POM File' field, and navigate to the pom.xml file located at the root of this project.
4. Select the Mule 3.8.1 Runtime, and click Finish.
5. This will import the project into the workspace for Anypoint Studio ready to compile and install.

## Running the tests

This project contains 30 Unit Tests in total that will test the functions of the API and associated subflows.

### Running MUnit Tests
In order to execute the unit tests of this project:
1. Right-Click on the project root folder > Mule > Configure Maven...
2. Check that the Maven installation directory is set, and the Base command is set to _**mvn clean test**_ and click OK.
3. Right-Click on the project root folder > Run As > Mule Application with Maven. This will begin to execute the Unit Tests for the project. Once completed, you should see something similar to the below at the end of the output:

		Printing Coverage Report...
        ===============================================================================
        MUnit Coverage Summary
        ===============================================================================
        * Resources: 1 - Flows: 15 - Message Processors: 36
        * Application Coverage: 97.22%
        ====================================================================================
        MUnit Run Summary                                                                   
        ====================================================================================
        >> myorganization-api-test.xml test result: Tests: 23, Errors: 0, Failures:0, Skipped: 0
        >> myorganization-subflow-test.xml test result: Tests: 7, Errors: 0, Failures:0, Skipped: 0

        ====================================================================================
        > Tests:   	30
        > Errors:  	0
        > Failures:	0
        > Skipped: 	0
        ====================================================================================
        ------------------------------------------------------------------------
        BUILD SUCCESS
        ------------------------------------------------------------------------

### Understanding the MUnit Tests
The RAML file was imported into Mule Anypoint Studio APIKit which created the necessary flows for the API resources and methods defined. In addition the associated subflows that would be assumed to be created to complete this API have been created, and left unimplemented. These subflows, are later mocked within the MUnit test cases, to simulate the behaviour that would be observed.

Each MUnit test, where appropriate, will have all the mock conditions defined upfront prior to the HTTP request being made by Mules HTTP Request component. Once the API execution has completed, the response and payload are later asserted.

The MUnit tests are separated into 2 main files:
1. /src/test/munit/myorganization-api-test.xml
This is the main MUnit XML for the MyOrganization API Customers resource. This will execute unit tests against each method/resource with varying conditions, utilising the sample input and outputs located under /src/test/resources/myorganizationapi/samples/customer.

2. /src/test/munit/myorganization-subflow-test.xml
This is the main Munit XML for the related subflows that have been defined. The unit tests included within this file are provided only as a skeleton as the actual sub-flows are unimplemented.

## Deploying MyOrganization API on Anypoint Studio Mule Embedded runtime

Similar to executing the unit test, to deploy the API we will utilise maven as follows:

1. Right-Click on the project root folder > Mule > Configure Maven...
2. Check that the Maven installation directory is set, and the Base command is set to _**mvn clean install**_ and click OK.
3. Right-Click on the project root folder > Run As > Mule Application with Maven. This will begin to execute the unit tests for the project, and once completed, begin the start-up of the API. Once completed, you should see the following API Landing Page:

![APILandingPage.png]({{site.baseurl}}/APILandingPage.png)


### Executing API Functions

The API Console starts up on localhost at the following: http://localhost:8081/console
From here the console page can be navigated to the various Customer resources that have been defined.
As the various APIs are tested/invoked you will see output in the Mule Anypoint Studio Console.
Example API requests that can be made from Postman/cURL as shown below.

**Retrieving Customer Records**

    curl -X GET \
      http://127.0.0.1:8081/api/customers \
      -H 'accept: application/json' \
      -H 'content-type: application/json'

**Creating a new Customer Record**

    curl -X POST \
      http://127.0.0.1:8081/api/customers \
      -H 'accept: application/json' \
      -H 'content-type: application/json' \
      -d '{
        "firstName": "John",
        "lastName": "Doe",
        "address": "123 Fake Street"}'

**Retrieving Customer Record by Id**

    curl -X GET \
      http://127.0.0.1:8081/api/customers/333 \
      -H 'accept: application/json' \
      -H 'content-type: application/json'

**Updating a Customer Record**

    curl -X PUT \
      http://127.0.0.1:8081/api/customers/333 \
      -H 'accept: application/json' \
      -H 'content-type: application/json' \
      -d '{
        "customerId": "333",
        "firstName": "John",
        "lastName": "Doe",
        "address": "700 Mars Road"}'

**Deleting a Customer Record**

    curl -X DELETE \
      http://127.0.0.1:8081/api/customers/333 \
      -H 'accept: application/json' \
      -H 'content-type: application/json'
