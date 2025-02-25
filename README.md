### Project: 
* Dynamically Increment Application version in Jenkins Pipeline

### Technologiesused: 
* Jenkins, Docker, GitLab, Git, Java, Maven 


### Project Description:
 * Software versioning consists of three parts: major, minor, and patch versions. In this project, we will use Jenkins as a CI/CD to automatically increment the version (jenkinsfile) in the stage as part of the code.



 * Increment version locally with maven build tool
  *  just we increment the patch version and keeps the major and minor version

#### command for increment version: 
  1 .```mvn build-helper:parse-version versions:set```:
   * ```build-helper:parse-version```: This goal parses the current project version to extract major, minor, and incremental version components (e.g., ```1.2.3``` → ```major=1```, ```minor=2```, ```incremental=3```).  
   * ```versions:set```: This goal sets a new version for the project in the ```pom.xml``` file.
   
 2. ```DnewVersion=${parsedVersion.majorVersion}.${parsedVersion.minorVersion}.${parsedVersion.nextIncrementalVersion}```:
   * This defines the new version to be set using the parsed components. ```${parsedVersion.majorVersion}```, ```${parsedVersion.minorVersion}```, and ```${parsedVersion.nextIncrementalVersion}``` are placeholders that will be replaced with actual values (e.g., ```1.2.4```).
    
3. ```versions:commit:```
   * This commits the version change to the ```pom.xml``` file.
   

  
  

<img src="https://user-images.githubusercontent.com/96679708/230752916-c63e6e37-013a-4ba2-b1d3-79b7b21f7591.png" width="700">

#### Configure CI step:Increment patch version as CI/CD pipeline 


<img src="https://github.com/user-attachments/assets/82e91a73-38ae-4ff6-b364-1d23b24f7ea8" width="700">


* Intergrate this ```Increment version``` → ```build app``` → ```Build image``` →  ```Push to  docker repository``` step of setting a new version  in pom.xml file into jenkins file so it's a part of build  pipeline 

* We need to Increment the version before application build so that we get jar file with correct incremented version
  
* Configured the Jenkins file to increment the version. 

* This script extracts the version number from a ```pom.xml``` file and uses it to create a Docker image name.
   *   ```readFile('pom.xml')```: Reads the content of the pom.xml file.
   * ```=~ '<version>(.+)</version>'```: Applies a regular expression to match the version tag in the pom.xml file and capture the version number.
   * ```matcher[0][1]```: Accesses the first match and gets the captured version number.
   * ```env.IMAGE_NAME = "$version-$BUILD_NUMBER"```:Sets the environment variable ```IMAGE_NAME``` by combining the extracted version number (version) with the build number (BUILD_NUMBER), separated by a hyphen.


 <img src="https://user-images.githubusercontent.com/96679708/230752979-8bfa6e02-a45c-429a-9e2f-7312550d42bf.png" width="700">
 
    

  ####   Configure CI step: Build Java application and clean old artifacts

   * Build Java application and clean old artifacts

   * ```maven clean package``` jenkins server will clean the target directory from the old build   and build the new application jar 


#### Configure CI step: Build Image with dynamic Docker Image Tag
* Every time we are releasing the new version, we are not actually releasing a jar file but rather a Docker image.

* Adjusted Dockerfile file by changing the hardcoded version and replacing it with regular expression

* Build a Docker image tag with dynamic images and use the application version for the Docker image version.

* The image name will be dynamic; we need to set it every time we run a build pipeline. 

* To access the value or variable of ```IMAGE_NAME```  by reading it from the ```pom.xml``` file
        
 <img src="https://user-images.githubusercontent.com/96679708/230753093-7d991eb3-8a05-4d7b-8adb-60964263ad6d.png" width="700">


  *  Executed Jenkins Pipeline

 

<img src="https://user-images.githubusercontent.com/96679708/230753069-b0fd735a-3ed7-45f2-80e0-00f9fe3410f5.png" width="700">

   
#### Configure CI step: Push Image to private DockerHub repository 

  * Push Image to private DockerHub repository  



<img src="https://github.com/Rajib-Mardi/Dynamically-Increment-Application-version-in-Jenkins-Pipeline/assets/96679708/0a751a37-4c37-4902-8205-12c3de71af08" width="700">


####  Configure CI step: Commit version update of Jenkins back to Git repository

  * This stage configures Git (with user details), checks the current repository status, and securely commits and pushes changes to the ```jenkins-jobs``` branch on GitLab, using credentials stored in Jenkins. The commit is made with the message ```"ci:jenkins-jobs"```.

  

<img src="https://github.com/user-attachments/assets/78d96df6-ef36-4f89-ad01-79ef166c32d3" width="700">


* Exectued the jenkins pipeline



<img src="https://user-images.githubusercontent.com/96679708/230757879-09ae27ce-88d2-48d1-a0fa-6c6fc2a4e36c.png" width="700">


 #### Configure Jenkins pipeline to not trigger automatically on CI build commit to avoid commit loop



 * When we trigger it automatically with Webhook, it increments the commit version in git. It will automatically trigger again and again, and this will continue to go on and on in loop because of the commit trigger.


 * Detect that commit was made from jenkins 

* Ignore the trigger when the commit is from Jenkins using the plugin "ignore committer strategy."
 
 



<img src="https://github.com/user-attachments/assets/abc5b945-a904-4a80-a0b4-8f45def64771" width="700">

 

 * In configuration, add the plugin, provide the author email for Jenkins users, and allow them to ignore the author and save it.



<img src="https://user-images.githubusercontent.com/96679708/230757827-4fc6e009-391b-4b2f-a383-a754d8fbf6ed.png" width="700">


* Make some changes in the git file and push it to the repository. 
* It will trigger automatically again, and after a commit happens, a new pipeline should not be triggered.


<img src="https://user-images.githubusercontent.com/96679708/230757879-09ae27ce-88d2-48d1-a0fa-6c6fc2a4e36c.png" width="700">




 <img src="https://user-images.githubusercontent.com/96679708/230757889-b9da0132-6076-4289-8359-5706332d4b30.png" width="700">

 
