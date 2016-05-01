1.Create an instance using AMI(Jenkins,docker,git,dockerimages)

2.Now have to check whether jenkins is up and running.Open 
                          <ipaddress>:8080
Jenkins dashboard will be opened now we have to manage plugins.Following are the plugins that are to be installed

3.Goto dashboard-> manage jenkins->manage plugins->available (install without restart)
  
  - Github Plugin
  - Docker plugin
 4. Open a terminal or create a job to check what images are present and containers are running : 

      docker images
      docker ps
      
5.Now our requirement is this job contains a sample ionic application which is developed using dockerfiles so if any change is happened to the code jenkins should help us to trigger and update our application.Following are the steps to be followed :
  
  
      1. Create a job in jenkins for example sample_application.
      2.Now configure in source code management select git :
           a.Add repositories where our sample ionic application is present and dockerfile
                - https://github.com/SusrithaMunukutla/Sensor-app.git 
                -https://github.com/samhitha30/Ionic_script.git
           b.Add credentails by clicking Add -> Include username,password,description .
      3.Now in Build trigger select Build when a change is pushed to GitHub, when any changes made to our git repository and commit,jenkins helps to trigger and build job with updated code.
      4.Now in Build -> Add build step -> Execute Shell
          a.Before executing this job make sure no container is running.
                  docker rm -f $(docker ps -aq)
                  docker run -i -d -p 8100:8100 --entrypoint=/entrypoint.sh <image-tag> -s
      5.Now save the job and build 
                       or
      Make any changes in your git repository and commit job gets triggered and builds.

Limitations :

 -> Before executing shell make sure all our containers are stopped and killed orelse will get  error when job is triggered.
                          
                           SPECIFIC PORT IS ALREADY ALLOCATED 

 -> If apk file has to be pushed to git following are the commands required.Open terminal->go into ionic container using
 
            docker exec -it <container-id> bash
            Then execute following commands and in between it prompts to give credentails 
                 -> enter your username and password 
            
                   git init
                   git add .
                   git commit -m "First commit"
                   git remote add testt https://github.com/samhitha30/Sensor-app.git
                   git push -f testt master

         
              
