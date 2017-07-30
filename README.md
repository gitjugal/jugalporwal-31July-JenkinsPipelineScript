# jugalporwal-31July-Repo2
This repository is contains Jenkins pipeline scipt file. 
The script is used for a parameterized Jenkins pipeline job with the following parameters : 

1)ProjectWorkspace - This parameter is used to specify the path of the java project which contains the gradle build script(The project can be clone from "https://github.com/gitjugal/jugalporwal-31July-Repo1.git"). Specify the path till root of gradle script.

2)ArtifactoryURL - The artifactory server URL. This URL is basically as - "http://{serverIP}/artifactory/api/build. Using a curl GET command on this url will fetch the details of all artifacts uploaded to the server and copy the corresponding output json to a file.

3)ApproverEmail - The email of the release manager who needs to be notified about the completion of packaging phase. It will send a link to this email which can be used to either approve or abort the consecutive job (i.e. Deployment). 

For task no. 3 , I used the "Parameterized Remote Trigger Plugin". Using this I passed on a parameter from a Jenkins server on one instance to a Jenkins server on another instance. It uses the credentials plugin to login to the remote server. Also the remote job needs to be paratemrized and the same parameter which is passed from the calling server should be defined. 
