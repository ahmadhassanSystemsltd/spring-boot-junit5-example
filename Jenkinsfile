pipeline {
    agent any
    
    stages {
//         stage('Checkout') {
//             steps {
//                 checkout scmGit(branches: [[name: '*/main']], extensions: [], userRemoteConfigs: [[url: 'https://github.com/mhassanmanzoorsl/spring-boot-jacoco.git']])
//             }
//         }

        stage('Build') {
            steps {
                sh 'mvn clean install'
                sh 'mvn clean package'
               // sh 'mvn clean package -Dmaven.test.failure.ignore=true'
            }
        }
        stage('Test') {
            steps {
                sh 'mvn test'
                //sh 'mvn test -Dmaven.test.failure.ignore=true'
            }
        }
        
        stage('Publishing Junit Tests report ') {
            steps {
                junit 'target/surefire-reports/*.xml'
            }   
        }
        stage('Publishing Code Covergae') {
            steps {
                jacoco()
            }   
        }
//         stage('Static Code Analysis') {
//            environment {
//              SONAR_URL = "http://10.100.122.100:9000"
//       }
//             steps {
//               withCredentials([string(credentialsId: 'sonarqube', variable: 'SONAR_AUTH_TOKEN')]) {
//                 sh 'cd src/ && mvn sonar:sonar -Dsonar.login=9c5f506736c32d024e07d4b5e4d3af76338f0458 -Dsonar.host.url=${http://10.100.122.100:9000}'
//         }
//       }
//     }
        stage('SonarQube Analysis') {
            steps {
                withSonarQubeEnv('sonarqube') {
                    sh '''
                        mvn clean install -DskipTests sonar:sonar -Dsonar.projectKey=demo -Dsonar.host.url=http://20.102.67.142:9000 -Dsonar.login=sqp_96389932750f69c14550ef4a5e2448cd19c4d93c
                    '''
                 }
             }
         }
        
//         stage('Docker') {
//             steps {
//                 sh 'docker version'
//                 sh 'docker build -t ahmedsystems/springbootjacoco:0.0.1 -f Dockerfile .'
//                 withDockerRegistry(credentialsId: 'ahmedsystems-dockerhub', url: 'https://index.docker.io/v1/') {
//                 sh 'docker push ahmedsystems/springbootjacoco:0.0.1'
//                 }
//              }
//          }
        
//         stage('Image push to local Docker registry') {
//             steps {
//                 sh 'docker version'
//                 sh 'docker build -t docker-registry:5000/springbootjacoco:0.0.1 -f Dockerfile .'
//                 withDockerRegistry(credentialsId: 'docker_reg_cred_k8', url:'https://docker-registry:5000/v2/') 
//                  {
//                  sh 'docker push docker-registry:5000/springbootjacoco:0.0.1'
//                  }
//              }
//          }
        // stage('Deploy to K8 Cluster') {
        //     steps {
        //         sshagent(['k8s_master_ssh_key']) {
        //             sh 'scp -r -o StrictHostKeyChecking=no springbootappk8s.yaml root@192.168.1.60:/'
        //             script{
        //             try{
        //                 sh 'ssh root@192.168.1.60 kubectl apply -f /springbootappk8s.yaml --kubeconfig=/root/.kube/config'
        //                }
        //             catch(error)
        //                {}
        //             }
    
        //         }
        //     }
        // }
    }
}
