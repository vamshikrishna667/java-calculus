node{
    stage('SCM Checkout'){
        git  'https://github.com/vamshikrishna667/java-calculus.git'
    }
    stage('MVN Package'){
        def mvnHome = tool name: 'maven-3', type: 'maven'
        def mvnCMD = "${mvnHome}/bin/mvn"
        sh '''${mvnCMD} mvn clean package
          '''
    }
    stage('Build Docker Image'){
    
     sh 'docker build -t vamshi2krishna/javacalculus2.0 . '
    }
    stage('Push docker image'){
       withCredentials([string(credentialsId: 'docker-PWD', variable: 'dockerhubPWD')])  {
    sh "docker login -u vamshi2krishna -p ${dockerhubPWD}"
    }

    sh 'docker push vamshi2krishna/javacalculus2.0 '
    }
    
      stage('Deploy on Dev server'){
        def dockerRun = 'docker run -p 8080:8080 -dt --name java-cal-2.0 vamshi2krishna/javacalculus2.0'
       sshagent(['dev -server']) {
       sh "ssh -o StrictHostKeyChecking=no centos@172.31.83.66 ${dockerRun}"
       }
    }   
    
    
}
