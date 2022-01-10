node{
   stage('SCM Checkout'){
     git 'https://github.com/Vasuvadivel/my-app.git'
   }
   stage('Compile-Package'){

      def mvnHome =  tool name: 'maven3', type: 'maven'   
      sh "${mvnHome}/bin/mvn clean package"
	  sh 'mv target/myweb*.war target/newapp.war'
   }
   stage('SonarQube Analysis') {
	        def mvnHome =  tool name: 'maven3', type: 'maven'
	        withSonarQubeEnv('sonar') { 
	          sh "${mvnHome}/bin/mvn sonar:sonar"
	        }
	    }
   stage('Build Docker Imager'){
   sh 'docker build -t vasuvadivel/cicd-project:1.0 .'
   }
   stage('Docker Image Push'){
   withCredentials([string(credentialsId: 'dockerpass', variable: 'dockerpassword')]) {
   sh "docker login -u vasuvadivel -p ${dockerPassword}"
    }
   sh 'docker push vasuvadivel/cicd-project:1.0'
   }
   
   stage('Remove Previous Container'){
	try{
		sh 'docker rm -f tomcattest'
	}catch(error){
		//  do nothing if there is an exception
	}
   stage('Docker deployment'){
   sh 'docker run -d -p 8100:8080 --name tomcattest saidamo/myweb:0.0.2' 
   }
}
}

