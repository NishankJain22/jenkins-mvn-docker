node{
  def ImageName = "suhvas/suhas-pridevops"
  def Creds	= "dockerhubaccount"
  def imageTag = "1.0"
  try{
		stage('Checkout'){
			  git 'https://github.com/suhvas/jenkins-mvn-docker.git'
			  //git 'https://github.com/maheshkharwadkar/mk-k8-ci-cd.git'
			  //sh "git rev-parse --short HEAD > .git/commit-id"
			  //imageTag= readFile('.git/commit-id').trim()
		}
		  //stage('RUN Unit Tests'){
		//	  sh "npm install"
		//	  sh "npm test"
		 // }
		stage('Docker Build, Push'){
			withDockerRegistry([credentialsId: "${Creds}", url: 'https://index.docker.io/v1/']) {
			   mvn install dockerfile:build 
			 //sh "docker build -t ${ImageName}:${imageTag} ."
			  //sh "docker push ${ImageName}"
			}
		}
		stage ('Push Docker image to DockerHub') {
		   // withCredentials([usernamePassword(credentialsId: 'amazon', usernameVariable: 'USERNAME', passwordVariable: 'PASSWORD')]) {
				withCredentials([string(credentialsId: "${Creds}", variable: "${Creds}")]) 
				{
				 sh "docker login -u suhvas -p ${dockerhubaccount}"
				}
				//sh 'docker push suhvas/suhas-pridevops:0.1.0'
				//sh "docker build ${ImageName}:${imageTag}"
			        sh "docker push ${ImageName}:${imageTag}"
				//sh 'docker rmi suhvas/suhas-pridevops:0.1.0'
				//sh "docker rmi -t ${ImageName}:${imageTag}"
		  //}
			//stage('Deploy on K8s'){
			//script{
			   // sh "cd ansible/sayarapp-deploy"
			//    sh "pwd"
			//    sh (
			//	    script: "cd ansible/sayarapp-deploy && ansible-playbook deploy.yml  --user=jenkins --extra-vars ImageName=${ImageName} --extra-vars imageTag=${imageTag} --extra-vars Namespace=${Namespace} -vv"
				//)  
			//  }
			//}
		}
    } catch (err) {
        currentBuild.result = 'FAILURE'
    }
	
}
