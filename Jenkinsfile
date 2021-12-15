node{
    def imgVersion = UUID.randomUUID().toString()
    def dockerImage = "nsivaaws/nodeapp:${imgVersion}"
    stage('Source Checkout'){
        
        git 'https://github.com/javahometech/node-app'
    }
    
    
    stage('Build Docker Image'){
        sh "docker build -t ${dockerImage} ."
    }
    
    stage('Push DockerHub'){
		withCredentials([string(credentialsId: 'docker-hub1', variable: 'dockerhubPwd')]) {
			sh "docker login -u nsivaaws -p ${dockerhubPwd}"
		}
        
        sh "docker push ${dockerImage}"
    }
    
	stage('Dev Deploy'){
		def dockerRun = "docker run -d -p 8080:8080 --name nodeapp ${dockerImage}"
		sshagent(['dev-docker']) {
		    try{
				sh "ssh -o StrictHostKeyChecking=no ec2-user@3.88.60.115 docker rm -f nodeapp "
			}catch(e){
			
			
			}
			sh "ssh  ec2-user@3.88.60.115 ${dockerRun}"
		}
	}
}
