# Vagrant Sonarqube 7.0

Vagrant configuration to run sonarqube.

## Intallation

1. Install Vagrant from https://www.vagrantup.com/downloads.html and virtual box https://www.virtualbox.org/
2. Ensure that vagrant is available via `vagrant -v`
3. Clone this repo locally
4. cd into this project folder and run the follwing command `vagrant up`
5. Once the VM is up the sonarqube instance will be running at http://localhost:9000  
    Default user/pass are admin/admin
6. Login and create a new project

## Run analysis on a project
1. Navigate to https://docs.sonarqube.org/display/SCAN/Analyzing+with+SonarQube+Scanner and donwload the scanner for your OS
2. cd into you source code project and run the `sonar-scanner` command.  
    On windows the command will look like this:  
    ```
    # replace the project key abd token
    sonar-scanner.bat -Dsonar.projectKey=<PROJECT KEY> -Dsonar.sources=. -Dsonar.host.url=http://localhost:9000 -Dsonar.login=<TOKEN>
    ```
    The token can be generated from Administration > Security > Users

**Note**: Usually sonarqube will require a database for storing all the anaylysis information.  
However if none is available it will use the embeded database (H2 locaded /data) 
which works just fine for tests and small projects.   
As a result to keep things simple this setup relies on the embeded database.

## Useful links
If you need to update the download links in the Vagrantfile you can get them from:

- Java JDK: http://www.oracle.com/technetwork/java/javase/downloads/index.html
- Sonarqube: https://www.sonarqube.org/downloads/

## Author
Mihai Ionut Vilcu
 
+ [github/ionutvmi](https://github.com/ionutvmi)
+ [twitter/ionutvmi](http://twitter.com/ionutvmi)

