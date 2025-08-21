pipeline {
    agent any
    
    parameters {
        choice(name: 'DEPLOY_ENV', choices: ['blue', 'green'], description: 'Choose which environment to deploy: Blue or Green')
        choice(name: 'DOCKER_TAG', choices: ['blue', 'green'], description: 'Choose the Docker image tag for the deployment')
        booleanParam(name: 'SWITCH_TRAFFIC', defaultValue: false, description: 'Switch traffic between Blue and Green')
    }

    tools {
        jdk 'jdk-17'
        maven 'maven-3'
    }
    environment {
        SCANNER_HOME = tool 'sonar-scanner'
        SONAR_TOKEN = credentials('sonar-token')
        IMAGE = "pj013525/bankapp"
        TAG = "${params.DOCKER_TAG}"
        K8S_NAMESPACE = "pj-ns"
    }

    stages {
        stage('Clean the Workspace') {
            steps {
                cleanWs()
            }
        }
        stage('Git Checkout') {
            steps {
                git branch: 'main', credentialsId: 'git-creds', url: 'https://github.com/pj013525/Blue-Green-Deployment.git'
            }
        }
        stage('Compile the code') {
            steps {
                sh 'mvn compile'
            }
        }
        stage('Trivy fs scan') {
            steps {
                sh 'trivy fs --format table -o fs-report.html .'
            }
        }
        stage('Code Quality Analysis') {
            steps {
                withSonarQubeEnv('sonar-server') {
                   sh """$SCANNER_HOME/bin/sonar-scanner -Dsonar.projectName=B-G-D \
                        -Dsonar.projectKey=B-G-D \
                        -Dsonar.java.binaries=target/classes \
                        -Dsonar.login=${env.SONAR_TOKEN} """
                }
            }
        }
        stage('Quality Analysis Report') {
            steps {
                timeout(time: 1, unit: 'HOURS') {
                   waitForQualityGate abortPipeline: false
                }
            }
        }
        stage('Packing the code') {
            steps {
                sh 'mvn package -DskipTests=true'
            }
        }
        stage('Publish to Nexus') {
            steps {
               withMaven(globalMavenSettingsConfig: 'maven', jdk: 'jdk-17', maven: 'maven-3', traceability: true) {
                  sh 'mvn deploy -DskipTests=true'
               }
            }
        }
        stage('Build and Tag the Image') {
            steps {
                script {
                    withDockerRegistry(credentialsId: 'docker-creds', toolName: 'docker') {
                        sh "docker build -t ${IMAGE}:${TAG} ."
                    }
                }
            }
        }
        stage('Trivy Image scan') {
            steps {
                sh 'trivy image --format table -o image-report.html ${IMAGE}:${TAG}'
            }
        }
        stage('Push the Image') {
            steps {
                script {
                    withDockerRegistry(credentialsId: 'docker-creds', toolName: 'docker') {
                        sh "docker push ${IMAGE}:${TAG}"
                        sh "docker image prune -f"
                    }
                }
            }
        }
        stage('Deploy mysql in K8S') {
            steps {
                script {
                    withKubeConfig(caCertificate: '', clusterName: 'devopsshack-cluster', contextName: '', credentialsId: 'K8S-creds', namespace: 'pj-ns', restrictKubeConfigAccess: false, serverUrl: 'https://8C2614C7D0E246BCEAD4B978D23F9EF4.gr7.ap-south-2.eks.amazonaws.com') {
                        sh "kubectl apply -f mysql-ds.yml -n ${K8S_NAMESPACE}"
                    }
                }
            }
        }
        stage('Deploy SVC-APP') {
            steps {
                script {
                    withKubeConfig(caCertificate: '', clusterName: 'devopsshack-cluster', contextName: '', credentialsId: 'K8S-creds', namespace: 'pj-ns', restrictKubeConfigAccess: false, serverUrl: 'https://8C2614C7D0E246BCEAD4B978D23F9EF4.gr7.ap-south-2.eks.amazonaws.com') {
                        sh """ if ! kubectl get svc bankapp-service -n ${K8S_NAMESPACE}; then
                                kubectl apply -f bankapp-service.yml -n ${K8S_NAMESPACE}
                              fi
                        """
                   }
                }
            }
        }
        
        stage('Deploy to Kubernetes') {
            steps {
                script {
                    def deploymentFile = ""
                    if (params.DEPLOY_ENV == 'blue') {
                        deploymentFile = 'app-deployment-blue.yml'
                    } else {
                        deploymentFile = 'app-deployment-green.yml'
                    }

                    withKubeConfig(caCertificate: '', clusterName: 'devopsshack-cluster', contextName: '', credentialsId: 'K8S-creds', namespace: 'pj-ns', restrictKubeConfigAccess: false, serverUrl: 'https://8C2614C7D0E246BCEAD4B978D23F9EF4.gr7.ap-south-2.eks.amazonaws.com') {
                        sh "kubectl apply -f ${deploymentFile} -n ${K8S_NAMESPACE}"
                    }
                }
            }
        }
        
        stage('Switch Traffic Between Blue & Green Environment') {
            when {
                expression { return params.SWITCH_TRAFFIC }
            }
            steps {
                script {
                    def newEnv = params.DEPLOY_ENV

                    // Always switch traffic based on DEPLOY_ENV
                    withKubeConfig(caCertificate: '', clusterName: 'devopsshack-cluster', contextName: '', credentialsId: 'K8S-creds', namespace: 'pj-ns', restrictKubeConfigAccess: false, serverUrl: 'https://8C2614C7D0E246BCEAD4B978D23F9EF4.gr7.ap-south-2.eks.amazonaws.com') {
                        sh '''
                            kubectl patch service bankapp-service -p "{\\"spec\\": {\\"selector\\": {\\"app\\": \\"bankapp\\", \\"version\\": \\"''' + newEnv + '''\\"}}}" -n ${K8S_NAMESPACE}
                        '''
                    }
                    echo "Traffic has been switched to the ${newEnv} environment."
                }
            }
        }
        
        stage('Verify Deployment') {
            steps {
                script {
                    def verifyEnv = params.DEPLOY_ENV
                    withKubeConfig(caCertificate: '', clusterName: 'devopsshack-cluster', contextName: '', credentialsId: 'K8S-creds', namespace: 'pj-ns', restrictKubeConfigAccess: false, serverUrl: 'https://8C2614C7D0E246BCEAD4B978D23F9EF4.gr7.ap-south-2.eks.amazonaws.com') {
                        sh """
                        kubectl get pods -l version=${verifyEnv} -n ${K8S_NAMESPACE}
                        kubectl get svc bankapp-service -n ${K8S_NAMESPACE}
                        """
                    }
                }
            }
        }
    }
}
