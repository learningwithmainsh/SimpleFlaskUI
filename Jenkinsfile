node(){
    def buildNumber = BUILD_NUMBER
    stage('Cloning Git') {
        git 'https://github.com/learningwithmainsh/SimpleFlaskUI.git'

    }
        
    
    stage('Docker Image build'){
    
    sh "docker build -t manishgenius/simpleflaskui:${buildNumber} ."

    
}

  stage('Docker login and Push'){
    withCredentials([string(credentialsId: 'Docker_hub_pwd', variable: 'DockerHubPwd')]) {
    // some block
sh "docker login  -u  manishgenius -p ${DockerHubPwd}"
}
    
    
    sh " docker push manishgenius/simpleflaskui:${buildNumber} "
}  
 stage('Deployment')
  
 {
sshagent(['Docker_deployment_server']) 
  
   {
       //sh "ssh -o StrictHostKeyChecking=no ubuntu@13.232.59.220 'echo $HOME'"
       
    sh "ssh -o StrictHostKeyChecking=no ubuntu@172.31.13.106 docker rm -f  simpleflaskui || true"
    
//sh "ssh -o StrictHostKeyChecking=no ubuntu@13.232.59.220 ls"
 sh "ssh -o StrictHostKeyChecking=no ubuntu@172.31.13.106 docker run -d -p 8081:8080 --name simpleflaskui manishgenius/simpleflaskui:${buildNumber}  "
}

 }
 
 
 stage('Email Alert')
 {
   
 emailext body: '''Build Over..

 Regards,
 Manish,
 8765368754''', subject: 'Build Over...', to: 'mnshkmrpnd@gmail.com'
 
 }
}

