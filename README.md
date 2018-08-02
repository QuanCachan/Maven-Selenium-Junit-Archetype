"# Maven-Selenium-Junit-Archetype" 

Introduction
============

This archetype generates a small Maven project with Selenium WebDriver and Junit Test embedded.

To install the archetype in your local repo:

	git clone https://github.com/QuanCachan/Maven-Selenium-Junit-Archetype.git
	cd Maven-Selenium-Junit-Archetype
	mvn install

Now, from the workspace you want to create a new Maven project using this archetype:
    
    Firstly; ensure that there's no pom.xml file in that directory.

    Then, you can type:

        mvn archetype:generate -DarchetypeGroupId=fr.henix.squash -DarchetypeArtifactId=maven-selenium-junit-archetype -DarchetypeVersion=0.0.1-SNAPSHOT -DgroupId=mygroupId -DartifactId=myartifactId
    						 
where *mygroupId* : group id of the project to be create; *myartifactId* : artifact id of the project to be created

It uses Java bindings for Selenium version 2.47.2 and Junit TestNG version 6.9.6.

Project Structure
-----------------------------------

The project follows the standard Maven structure, so all the tests go in the *src/test/java* folder.  Tests should inherit from the **TestBase** class. In this class a factory method
from **WebDriverFactory** class is in charge of generating the instance of the WebDriver interface you need. Different parameters are passed into the factory:

* base URL : base URL of the AUT (application under test)
* Grid 2 hub URL :  URL of the hub (if using Grid 2)
* browser features: a) name b) version c) platform
* username / password : in case of BASIC authenticated site

Those parameters are retrieved from the *src/main/resources/application.properties* file. You can also populate the properties file from command line (through -D<property in mvn command or through
Hudson/Jenkins).

**TestBase** class provides 30 seconds as interval for polling element from the DOM (implicity wait), and also it takes care of closing the driver when all the tests are executed in the suite. 
(Feel free to update all this values according to your needs)

**HomePageTest** class (in *src/test/java/pages*) is just an example of a test class for testing the homepage of a web application. This test class accepts the *path* parameter
 (set in *src/test/resources/testng.xml)* defining the path appended to the base URL (in case of the home page, usually just "/"). In the setup method of this class, the **PageFactory** class is used
 to help supporting the **PageObject** pattern (see below for more information). Briefly according to this pattern, each page is an object. *src/main/java/pages/HomePage* class is an example of 
 a class representing the home page. Notice how the constructor accepts the "WebDriver" interface as parameter and all the "services" available for that page should be exposed here. It also allows to
 decouple the DOM element from the functionalities offered by the page.
 
 
In the TestBase class, I commented out the method launched after the suite runs and it takes screenshots in case of failure. Feel free to uncomment it out if you need and if your driver supports that
funcitonality. (at the moment it's not supported on mobile devices)


Adding Chrome Driver to the project
-----------------------------------

If you need to use chromedriver, you should put the proper driver file downloaded from http://code.google.com/p/chromium/downloads/list into *src/main/resources/drivers/chrome*. If you are on Windows, the file should be named *chromedriver.exe*,
if on Unix-based system, the file should be named *chromedriver*.


TestNG
------
For more info around TestNG framework, go to http://testng.org/doc/index.html. If you prefer, you could substitute this framework with JUnit.


Page Object pattern
-------------------
For more info around this pattern, read this wiki page: https://github.com/SeleniumHQ/selenium/wiki/PageObjects


Integration with SauceLabs
-------------------
You can easily integrate the project with SauceLabs, great service to launch tests in the cloud. You need to retrieve your SauceLab key and setting the "grid2.hub" property with thid syntax:
*http://<username>:<token>@ondemand.saucelabs.com:80/wd/hub* 

Further Notes
-------
The project is just a starting point, feel free to modify it according to your needs. 

Credits
-------
The Maven-Selenium-Junit-Archetype project is an open source project licensed under the Apache License 2.0.