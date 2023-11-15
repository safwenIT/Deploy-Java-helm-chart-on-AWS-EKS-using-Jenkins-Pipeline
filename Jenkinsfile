pipeline {
  agent any
    
  stages {
    stage("Clone code from GitHub") {
            steps {
                script {
                    checkout scmGit(branches: [[name: '*/main']], extensions: [], userRemoteConfigs: [[credentialsId: 'GITHUB_CREDENTIALS', url: 'https://github.com/safwenIT/Deploy-Java-helm-chart-on-AWS-EKS-using-Jenkins-Pipeline.git/']])
                }
            }
        }



   stage('mvn') {
            steps {               
                  sh 'mvn package'
                }
            }     
  
     
    stage('build java Docker Image') {
            steps {
                script {
                  sh 'docker build -t safwenit/formation-devops .'
                }
            }
        }


    stage('Deploy Docker Image to DockerHub') {
            steps {
                script {
                    withCredentials([string(credentialsId: safwenit, variable: 'safwenit')]) {
                        sh "docker login -u   safwenit   -p ${safwenit}"
                    }
                    sh "docker push safwenit/formation-devops"
        }
            }   
        }
         
     stage('Deploying java helm chart on eks') {
      steps {
        script {
          sh ('aws eks update-kubeconfig --name sample --region ap-south-1')
          sh "kubectl get ns"
          sh "helm install java ./java-chart"
        }
      }
    }

  }
}
