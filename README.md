### Project: 
* Dynamically Increment Application version in Jenkins Pipeline

## Technologiesused: 
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
   

  
  ![MINGW64__c_Users_Rajib_java-maven-app 08-04-2023 13_35_16](https://user-images.githubusercontent.com/96679708/230752916-c63e6e37-013a-4ba2-b1d3-79b7b21f7591.png)


### Configure CI step:Increment patch version as CI/CD pipeline 

![16 - Dynamically Increment Application version in Jenkins Pipeline - - Brave 18-11-2024 01_47_25](https://github.com/user-attachments/assets/82e91a73-38ae-4ff6-b364-1d23b24f7ea8)


* Intergrate this ```Increment version``` → ```build app``` → ```Build image``` →  ```Push to  docker repository``` step of setting a new version  in pom.xml file into jenkins file so it's a part of build  pipeline 
  * Increment version in Jenkins Pipeline

    * Configured Jenkinsfile to increment version
      * stage  :increment version
      * increment  patch version in pom.xml
      * read new version from pom.xml


   
    ![java-maven-app – Jenkinsfile 08-04-2023 14_11_05](https://user-images.githubusercontent.com/96679708/230752979-8bfa6e02-a45c-429a-9e2f-7312550d42bf.png)

    
    
    
    

   * Build Java application and clean old artifacts

      * ```maven clean package``` jenkins server will clean the target directory from the old build   and build the neew application jar 

     * Adjusted Dockerfile file 

       *  Build docker  with dynamic Docker Image Tag
        * build docker image from dockerfile
        * tag image with repository URL  and name 
        
        
        
     ![java-maven-app – Jenkinsfile 08-04-2023 14_17_55](https://user-images.githubusercontent.com/96679708/230753093-7d991eb3-8a05-4d7b-8adb-60964263ad6d.png)


     * Push Image to private DockerHub repository
       

   ![rajibmardi_my-repo Tags _ Docker Hub - Brave 17-10-2023 17_45_12](https://github.com/Rajib-Mardi/Dynamically-Increment-Application-version-in-Jenkins-Pipeline/assets/96679708/0a751a37-4c37-4902-8205-12c3de71af08)

     
   

     


  *  Executed Jenkins Pipeline

  *  build the jenkins-jobs pipline
 
 ![Screenshot 2023-04-08 173822](https://user-images.githubusercontent.com/96679708/230753069-b0fd735a-3ed7-45f2-80e0-00f9fe3410f5.png)

   
 ### Configure Jenkins pipeline to not trigger automatically on CI build commit to avoid commit loop



 * when we trigger automatically with webhook it Increment the commit version in git , it will automatically trigger again and again and this will continue to go and on in loop because of the commit trigger.


 * Detect that commit was made from jenkins 
 * Ignore the trigger when commit is from jenkins using the plugin "ignore committer  Strategy"


 
 
 ![Installed plugins - Plugin Manager  Jenkins  - Google Chrome 09-04-2023 11_44_54](https://user-images.githubusercontent.com/96679708/230757766-b2e9316b-e9a6-4ce1-9fc0-63851358d259.png)

 

 * In configuration add the plugin provide the author email for jenkins user and allow the ignore author and save it.

![Screenshot 2023-04-08 215211](https://user-images.githubusercontent.com/96679708/230757827-4fc6e009-391b-4b2f-a383-a754d8fbf6ed.png)



 * make some change in jenkinsfile and push it to the repository 
 * it will trigger automatically again and after commit happens ,a new pipeline should not triggered.



 
 ![Screenshot 2023-04-08 220722](https://user-images.githubusercontent.com/96679708/230757879-09ae27ce-88d2-48d1-a0fa-6c6fc2a4e36c.png)


 
 ![Screenshot 2023-04-08 220912](https://user-images.githubusercontent.com/96679708/230757889-b9da0132-6076-4289-8359-5706332d4b30.png)

 
 
