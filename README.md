## Demo Project: 
* Dynamically Increment Application version in Jenkins Pipeline

## Technologiesused: 
* Jenkins, Docker, GitLab, Git, Java, Maven 


Project Description:

Configure CI step:Increment patch version

  * Increment version locally with maven build tool
  

  
  ![MINGW64__c_Users_Rajib_java-maven-app 08-04-2023 13_35_16](https://user-images.githubusercontent.com/96679708/230752916-c63e6e37-013a-4ba2-b1d3-79b7b21f7591.png)



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

    
     
     ![Screenshot 2023-04-08 173822](https://user-images.githubusercontent.com/96679708/230753049-943c73be-9475-4d09-8642-84d9ed2f6ae6.png)

     


     Executed Jenkins Pipeline

     * build the jenkins-jobs pipline
 
 ![Screenshot 2023-04-08 173822](https://user-images.githubusercontent.com/96679708/230753069-b0fd735a-3ed7-45f2-80e0-00f9fe3410f5.png)

   
