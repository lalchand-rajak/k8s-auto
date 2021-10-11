pipeline {
    agent any

    tools {
        maven "mymvn"
        //jdk "JDK"
    }
    environment{
        dockerImage = ''
        registry= 'lalchandrajak05/myapp'
        registryCredential= 'dockerhub'
    }

    stages {
        // stage('Initialize'){
        //     steps{
        //         echo "PATH = ${MAVEN_HOME}/bin:${PATH}"
        //         echo "MAVEN_HOME = /usr/share/maven"
        //     }
        // }
        stage('Build') {
            steps {
               // dir("/var/lib/jenkins/workspace/test-k8s-auto/my-app/") {
                sh 'mvn -B -DskipTests clean package'
                }
            
            }
        
       stage('Build Images') {
       steps {
        //sh 'mvn clean package'
        script {
          //docker.withRegistry('https://hub.docker.com/repository/docker/lalchandrajak05/test-k8s') {
           // def image = docker.build("my-app")
           dockerImage = docker.build registry
          }
             }

        }
        stage('uploading to docker image'){
            steps{
                script{
                    docker.withRegistry( '', registryCredential ) {
                     dockerImage.push()
                    }

                }
            }
        }
        stage('k8s deploy'){
        steps {
            sshagent(['k8s deploy']) {
            sh "scp -o StrictHostKeyChecking=no k8s-auto.yml root@10.210.0.133:/root"
                script{
                         //try{
                        sh "ssh root@10.210.0.133 kubectl apply -f k8s-auto.yml"
                        //  }catch(error){
                       sh "ssh root@10.210.0.133 kubectl rollout restart deployment test-app"
                         //   }
    
                    }
            }
         }
       }

    }
    
}
