node{
  stage ('scm checkout') {
      git ('https://github.com/suhvas/jenkins-mvn-docker.git')
  }

  stage ('docker image build') {
     sh 'mvn clean install dockerfile:build '
  }

  stage ('Push Docker image to DockerHub') {
   // withCredentials([usernamePassword(credentialsId: 'amazon', usernameVariable: 'USERNAME', passwordVariable: 'PASSWORD')]) {
    withCredentials([string(credentialsId: 'dockerhubaccount', variable: 'dockerhubaccount')]) 
    {
     sh "docker login -u suhvas -p ${dockerhubaccount}"
      //sh "docker login -u suhvas -p Vastre@123"
    }
    sh 'docker push suhvas/suhas-pridevops:0.1.0'
    sh 'docker rmi suhvas/suhas-pridevops:0.1.0'
  }
  
  stage ('Deploy to Dev') {
		def dockerRun = 'docker run -p 8080:8080 -t suhvas/suhas-pridevops:0.1.0'
		/*sshagent(['deploy-to-dev-docker']) {
			sh "ssh -o StrictHostKeyChecking=no suhvas@100.26.227.218 ${dockerRun}"
		}*/
    sh "${dockerRun}"
	}
  
  stage ('Test') {
    sh "curl http//localhost:8080"
  }
}
