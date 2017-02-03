[![Build Status](https://travis-ci.org/belgattitude/php-java-bridge.svg?branch=master)](https://travis-ci.org/belgattitude/php-java-bridge)
[![License: MIT](https://img.shields.io/badge/License-MIT-yellow.svg)](https://opensource.org/licenses/MIT)

# Unofficial fork of the [PHP/Java bridge](http://php-java-bridge.sourceforge.net/pjb/) server.

The VM Bridge is a network protocol which can be used to connect a native 
script engine, for example PHP, with a Java VM. To get more information have a 
look to those projects:

- [soluble-japha](https://github.com/belgattitude/soluble-japha) PHP client to interact with the bridge.
- [pjb-starter-gradle](https://github.com/belgattitude/pjb-starter-gradle) Modern starter project to quickly configure and run PHPJavaBridge servers. 
- [pjbserver-tools](https://github.com/belgattitude/pjbserver-tools) A standalone server for helping php unit tests on travis.

## Disclaimer

> This fork have been made on the *no-longer maintained* sourceforge [CVS PHP/Java bridge repository](https://sourceforge.net/p/php-java-bridge/code/) and
> is based on the latest official release (6.2.1) excluding all previous versions and history (cleanup from the CVS repo).
> See the [CHANGELOG.md](https://github.com/belgattitude/php-java-bridge/blob/master/CHANGELOG.md) or have a look to the [CHANGESET](https://github.com/belgattitude/php-java-bridge/compare/Original-6.2.1...master).
> A copy of the original 6.2.1 release is available on a [separate branch](https://github.com/belgattitude/php-java-bridge/tree/Original-6.2.1). All new changes are currently made on the master branch, releases starting at 6.2.10.

## Status

Latest version 6.2.1 has been released long ago but, AFAIK, proved stable and mature. Here are some plans and statuses of the fork:  

- [x] Migration from CVS to github
- [x] Support for PHP7 and rewrite of the client `Java.inc`, see [soluble-japha](https://github.com/belgattitude/soluble-japha)
- [x] Test and update of the build file, Tomcat8 and JDK8.
- [x] Releasing a new version, with downloadable releases on Github (6.2.10)
- [x] Prepare a starter project with servlet 3.0 spec support, see [pjb-starter-gradle](https://github.com/belgattitude/pjb-starter-gradle).
- [x] Preliminary support and conversion to gradle (with ant tasks).
- [x] Regenerate and host [Java API doc](http://docs.soluble.io/php-java-bridge/api)
- [ ] Port and convert most of `./test.php5` in [soluble-japha](https://github.com/belgattitude/soluble-japha).
- [ ] License issue; MIT or GPL (website indicates MIT while CVS GPL) ? Try to contact original developers.
- [ ] Remove dependency of php in build scripts (should be possible to build without php) 
- [ ] Deprecate and remove completely the `Java.inc` client.
- [ ] Publish on maven (need help - must be made after removal of php dependency)
- [ ] Removal of obsolete code and resources.
- [ ] Security review and safe practices.
- [ ] Write JUnit tests
- [ ] Start refactorings and improve ;)
- [ ] Documentation (always a wip)

- Master branch: [![Build Status](https://travis-ci.org/belgattitude/php-java-bridge.svg?branch=master)](https://travis-ci.org/belgattitude/php-java-bridge)
- Develop branch: [![Build Status](https://travis-ci.org/belgattitude/php-java-bridge.svg?branch=develop)](https://travis-ci.org/belgattitude/php-java-bridge)

Please **[participate in the discussion for future ideas here](https://github.com/belgattitude/php-java-bridge/issues/6)**. 

## Documentation
   
- [API doc](http://docs.soluble.io/php-java-bridge/api).
- Older documentation can be found in the [PHP/Java bridge](http://php-java-bridge.sourceforge.net/pjb/) site

## Releases

- You can download pre-compiled [java bridge binaries](https://github.com/belgattitude/php-java-bridge/releases) on the releases page (jdk8). 
- Alternatively you can build the project, first clone the project and follow the build steps.
- [Gradle starter project](https://github.com/belgattitude/pjb-starter-gradle) on it's way !!! 

## Build the project

### Requirements

 - Oracle JDK 7,8
 - Gradle and Ant installed
 - PHP CLI >= 5.3, < 7.0, [see #4](https://github.com/belgattitude/php-java-bridge/issues/4) 
 
### Clone the project

Run the `git clone` command clone in a directory:

```shell
$ git clone https://github.com/belgattitude/php-java-bridge.git
$ cd php-java-bridge
```

### Build 

> Building the project requires a php interpreter (5.3 - 5.6) installed. By default
> it will use the `php` found in system path, but you can specify another location through the `-Dphp_exec=` argument.  

```
$ gradle build 
```

### Generated files

See the `/build/libs` folder :

| File          | Description   | Approx. size |
| ------------- | ------------- | ------------ |
| `JavaBridge-<VERSION>.jar`  | JavaBridge library. | +/- 500k |
| `JavaBridge-<VERSION>.war`  | Ready to deploy war template file. | +/- 900k |
| `JavaBridge-<VERSION>-sources.jar`  | JavaBridge sources. | +/- 400k |
| `JavaBridge-<VERSION>-javadoc.jar`  | Generated API documentation. | +/- 600k |
       

## Develop
                                                         
### Gradle tasks

```shell
$ gradle tomcatRun
$ # gradle tomcatStop (to stop the server)
```
                   
                                     
## Usage

### Deploy

> Currently only tested on Tomcat 7/8, should be running on any servlet 2.5 compatible container.

#### Deploy on Tomcat8 

Ensure you have tomcat installed and a php-cgi

```shell
$ sudo apt-get install tomcat8 tomcat8-admin
$ sudo apt-get install php-cgi
```

And copy the ready to run `JavaBridge-<VERSION>.war` in the tomcat webapps folder:

```shell
$ cp build/libs/JavaBridge-<VERSION>.war /var/lib/tomcat8/webapps/JavaBridge.war
```

Wait few seconds for deployment and point your browser to [http://localhost:8080/JavaBridge](http://localhost:8080/JavaBridge).

Have a look to the error log if needed:

```shell
$ cat /var/log/tomcat8/catalina.out
```

## FAQ

### OutOfMemory errors under Tomcat

If you get OutOfMemory errors, you can increase the java heap tomcat:

```shell
$ vi /etc/default/tomcat8
```

Look for the Xmx default at 128m and increase 

```
JAVA_OPTS="-Djava.awt.headless=true -Xmx512m -XX:+UseConcMarkSweepGC"
```

and restart

```shell
$ sudo service tomcat8 restart
```
 
## Contribute

Feel free to fork and submit pull requests :)

## Credits

See the [CREDITS.md](./CREDITS.md) for an updated list of contributions.

* Java refactorings and modernizations made by [Christian P. Lerch](https://github.com/cplerch)
* Fork initially made by [Sébastien Vanvelthem](https://github.com/belgattitude).

Original developers:

- Andre Felipe Machado, 
- Sam Ruby, 
- Kai Londenberg, 
- Nandika Jayawardana, 
- Sanka Samaranayake, 
- Jost Boekemeier