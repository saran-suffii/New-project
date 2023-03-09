# install jenkins
refer to official page of jenkins installation for desired os
important system ctl commands

# install maven
go to official website and download binary file and download to /opt dir
wget
tar -xvzf

# install docker
yum install docker -y
service docker start

# install maven plugin
configure maven-> add path on global tool configurator


# jenkins file
node{
   stage('SCM Checkout'){
     git 'https://github.com/saran-suffii/my-app.git'
   }
    stage('maven-buildstage'){

      def mvnHome =  tool name: 'maven3', type: 'maven'   
      sh "${mvnHome}/bin/mvn clean package"
	  sh 'mv target/myweb*.war target/newapp.war'
   }
   stage('SonarQube Analysis') {
	        def mvnHome =  tool name: 'maven3', type: 'maven'
	        withSonarQubeEnv('mysonar') { 
	          sh "${mvnHome}/bin/mvn sonar:sonar"
	        }
	    }
    stage('Build Docker Image'){
        sh 'docker build -t saransuffi/myweb:0.0.2 .'
   }
   stage('Docker Image Push'){
        withCredentials([string(credentialsId: 'saran_dock', variable: 'dockerPassword')]) {
            sh "docker login -u saransuffi -p ${dockerPassword}"
        }
    sh 'docker push saransuffi/myweb:0.0.2'
   }
    stage('Nexus Image Push'){
     sh "docker login -u admin -p admin123 100.26.28.164:8083"
     sh "docker tag saransuffi/myweb:0.0.2 100.26.28.164:8083/saransuffi:1.0.0"
     sh 'docker push 100.26.28.164:8083/saransuffi:1.0.0'
   }
   stage('Remove Previous Container'){
    	try{
	    	sh 'docker rm -f tomcattest'
    	}catch(error){
		//  do nothing if there is an exception
    	}
    }
    stage('Docker deployment'){
        sh 'docker run -d -p 8090:8080 --name tomcattest saransuffi/myweb:0.0.2' 
   }
   
}