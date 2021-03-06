 <a name="#documentr_top"></a>

> **This project requires JVM version of at least 1.7**






<a name="documentr_heading_0"></a>

# routemaster-module-templar-handler <sup><sup>[top](documentr_top)</sup></sup>



> Templar templating language module support for routemaster






<a name="documentr_heading_1"></a>

# Table of Contents <sup><sup>[top](documentr_top)</sup></sup>



 - [routemaster-module-templar-handler](#documentr_heading_0)
 - [Table of Contents](#documentr_heading_1)
 - [Creating a new module](#documentr_heading_2)
   - [Setup](#documentr_heading_3)
   - [Configuration](#documentr_heading_4)
   - [Code](#documentr_heading_7)
 - [Deploying modules](#documentr_heading_9)
   - [Step 1](#documentr_heading_10)
   - [Step 2](#documentr_heading_11)
   - [Step 3](#documentr_heading_12)
 - [Running the server](#documentr_heading_13)
 - [Building the Package](#documentr_heading_14)
   - [*NIX/Mac OS X](#documentr_heading_15)
   - [Windows](#documentr_heading_16)
 - [Running the Tests](#documentr_heading_17)
   - [*NIX/Mac OS X](#documentr_heading_18)
   - [Windows](#documentr_heading_19)
   - [Dependencies - Gradle](#documentr_heading_20)
   - [Dependencies - Maven](#documentr_heading_21)
   - [Dependencies - Downloads](#documentr_heading_22)


# Module Support

This is an example module that offers additional, modular support for [synapticloop routemaster](https://github.com/synapticloop/routemaster).

Routemaster modules can be automatically loaded from the `modules` directory that 



<a name="documentr_heading_2"></a>

# Creating a new module <sup><sup>[top](documentr_top)</sup></sup>



<a name="documentr_heading_3"></a>

## Setup <sup><sup>[top](documentr_top)</sup></sup>

Download the latest version of the [source code](https://github.com/synapticloop/routemaster-module-example/archive/master.zip), and unzip it into your project working directory.

The structure of the folder is as follows:

 - `build.gradle` (the build.gradle file)
 - `gradle`
 - `gradlew`
 - `gradlew.bat`
 - `settings.gradle` (the settings file for gradle)
 - `src` (the source files directory)




<a name="documentr_heading_4"></a>

## Configuration <sup><sup>[top](documentr_top)</sup></sup>

### File: `build.gradle`

This is the main build file and contains everything to build the module.  It can be executed by typing the following:

`gradle build`

If you are on windows:

`gradle.bat build`

The file contents are:




```
plugins {
	id 'java'
	id 'eclipse'
	id "maven"
	id "maven-publish"

	id "com.github.ben-manes.versions" version "0.13.0"
	id 'net.saliman.cobertura' version '2.2.6'
	id 'co.riiid.gradle' version '0.4.2'

	id "synapticloop.copyrightr" version "1.1.2"
	id "synapticloop.documentr" version "2.9.0"
}

version = '1.0.0'

//
// ensure that you change the following values
//
group = 'synapticloop'
archivesBaseName = 'routemaster-module-templar-handler'
description = """Templar templating language module support for routemaster"""

sourceCompatibility = 1.7
targetCompatibility = 1.7

repositories {
	mavenLocal()
	mavenCentral()
	jcenter()
}

dependencies {
	runtime 'synapticloop:routemaster:2.1.0'

	compile 'synapticloop:routemaster:2.1.0'
}

github {
	owner = group
	repo = archivesBaseName
	if(System.getenv('GITHUB_TOKEN')) {
		token = System.getenv('GITHUB_TOKEN')
	}
	tagName = version
	name = version
	assets = [
		'build/libs/' + archivesBaseName + '-' + version + '.jar',
	]
}

```


### File: `settings.gradle`

You will also want to change the `settings.gradle` file:




```
rootProject.name = 'routemaster-module-templar-handler'

```



so that the `rootProject.name` value matches your project name.




<a name="documentr_heading_7"></a>

## Code <sup><sup>[top](documentr_top)</sup></sup>

All the code resides in the `src/main/java` directory, with resources in the `src/main/resources` directory.

### File: `src/main/resources/routemaster.properties`

You **MUST** have a `routemaster.properties` file deployed with every module jar or it will not be activated.

The `routemaster.properties` file has the same standard layout as the default properties file, and may map options, handlers and routes (both RESTful and not).

Some things to note:

> All properties defined in the module jar file will **over-write** any existing properties that are set.

The `rouetmaster.properties` file deployed with this module is as follows:





```
option.indexfiles=index.html.templar,index.html,index.htm

# Now for some handlers - this is for the templar handle (bound to *.templar)

handler.templar=synapticloop.nanohttpd.handler.TemplarHandler

```






<a name="documentr_heading_9"></a>

# Deploying modules <sup><sup>[top](documentr_top)</sup></sup>



<a name="documentr_heading_10"></a>

## Step 1 <sup><sup>[top](documentr_top)</sup></sup>

To deploy the built module, ensure that you have downloaded the latest routemaster server from  [github routemaster releases](https://github.com/synapticloop/routemaster/releases) *(the jar file is the one that has the `server` classifier - e.g. `routemaster-2.0.0-server.jar`)*.

**Note:** The server version has **NO** functionality built in - i.e. no routes are defined - instead all functionality must come from the modules.  It will still operate successfully, however all the modules must map the routes.



<a name="documentr_heading_11"></a>

## Step 2 <sup><sup>[top](documentr_top)</sup></sup>

Create a `modules` directory which is in the same directory that the downloaded file is (your directory listing should look something like the following:




```
/routemaster-2.0.0-server.jar
/modules/
```





<a name="documentr_heading_12"></a>

## Step 3 <sup><sup>[top](documentr_top)</sup></sup>

Place the module (or multiple modules) into the `modules` directory so that it will look something like the following:



```
/routemaster-2.0.0-server.jar
/modules/
        /routemaster-module-example-1.0.0.jar
```





<a name="documentr_heading_13"></a>

# Running the server <sup><sup>[top](documentr_top)</sup></sup>

now run the server:

`java -jar routemaster-2.0.0-server.jar`

For the example above, the only routes that are mapped are: 

 - `/module/example/`
 - `/module/example/*`

If you open [http://localhost:5474/module/example/](http://localhost:5474/module/example/)

you will see something along the lines of:



```
Hello from the example module, this page was brought to you by the letter 'H'
```






<a name="documentr_heading_14"></a>

# Building the Package <sup><sup>[top](documentr_top)</sup></sup>



<a name="documentr_heading_15"></a>

## *NIX/Mac OS X <sup><sup>[top](documentr_top)</sup></sup>

From the root of the project, simply run

`./gradlew build`




<a name="documentr_heading_16"></a>

## Windows <sup><sup>[top](documentr_top)</sup></sup>

`./gradlew.bat build`


This will compile and assemble the artefacts into the `build/libs/` directory.

Note that this may also run tests (if applicable see the Testing notes)



<a name="documentr_heading_17"></a>

# Running the Tests <sup><sup>[top](documentr_top)</sup></sup>



<a name="documentr_heading_18"></a>

## *NIX/Mac OS X <sup><sup>[top](documentr_top)</sup></sup>

From the root of the project, simply run

`gradle --info test`

if you do not have gradle installed, try:

`gradlew --info test`



<a name="documentr_heading_19"></a>

## Windows <sup><sup>[top](documentr_top)</sup></sup>

From the root of the project, simply run

`gradle --info test`

if you do not have gradle installed, try:

`./gradlew.bat --info test`


The `--info` switch will also output logging for the tests



<a name="documentr_heading_20"></a>

## Dependencies - Gradle <sup><sup>[top](documentr_top)</sup></sup>



```
dependencies {
	runtime(group: 'synapticloop', name: 'routemaster-module-templar-handler', version: '1.0.0', ext: 'jar')

	compile(group: 'synapticloop', name: 'routemaster-module-templar-handler', version: '1.0.0', ext: 'jar')
}
```



or, more simply for versions of gradle greater than 2.1



```
dependencies {
	runtime 'synapticloop:routemaster-module-templar-handler:1.0.0'

	compile 'synapticloop:routemaster-module-templar-handler:1.0.0'
}
```





<a name="documentr_heading_21"></a>

## Dependencies - Maven <sup><sup>[top](documentr_top)</sup></sup>



```
<dependency>
	<groupId>synapticloop</groupId>
	<artifactId>routemaster-module-templar-handler</artifactId>
	<version>1.0.0</version>
	<type>jar</type>
</dependency>
```





<a name="documentr_heading_22"></a>

## Dependencies - Downloads <sup><sup>[top](documentr_top)</sup></sup>


You will also need to download the following dependencies:



### cobertura dependencies

  - `net.sourceforge.cobertura:cobertura:2.0.3`: (It may be available on one of: [bintray](https://bintray.com/net.sourceforge.cobertura/maven/cobertura/2.0.3/view#files/net.sourceforge.cobertura/cobertura/2.0.3) [mvn central](http://search.maven.org/#artifactdetails|net.sourceforge.cobertura|cobertura|2.0.3|jar))


### compile dependencies

  - `synapticloop:routemaster:2.1.0`: (It may be available on one of: [bintray](https://bintray.com/synapticloop/maven/routemaster/2.1.0/view#files/synapticloop/routemaster/2.1.0) [mvn central](http://search.maven.org/#artifactdetails|synapticloop|routemaster|2.1.0|jar))


### runtime dependencies

  - `synapticloop:routemaster:2.1.0`: (It may be available on one of: [bintray](https://bintray.com/synapticloop/maven/routemaster/2.1.0/view#files/synapticloop/routemaster/2.1.0) [mvn central](http://search.maven.org/#artifactdetails|synapticloop|routemaster|2.1.0|jar))

**NOTE:** You may need to download any dependencies of the above dependencies in turn (i.e. the transitive dependencies)

--

> `This README.md file was hand-crafted with care utilising synapticloop`[`templar`](https://github.com/synapticloop/templar/)`->`[`documentr`](https://github.com/synapticloop/documentr/)

--
