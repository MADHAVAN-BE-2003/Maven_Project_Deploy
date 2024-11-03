pipeline {
    agent any

    options {
        retry(3)
        timeout(time: 1, unit: 'HOURS')
    }

    stages {
        stage('Prepare Workspace') {
            steps {
                script {
                    try {
                        deleteDir()
                        echo 'Workspace cleaned successfully.'
                    } catch (Exception e) {
                        echo "Failed to clean workspace: ${e.message}"
                    }
                }
            }
        }
        
        stage('Checkout Code') {
            steps {
                script {
                    checkout([
                        $class: 'GitSCM', 
                        branches: [[name: '*/main']],
                        userRemoteConfigs: [[url: 'https://github.com/MADHAVAN-BE-2003/Maven_Project_Deploy.git']],
                        extensions: [[$class: 'CleanBeforeCheckout']]
                    ])
                    echo 'Code checkout successful.'
                }
            }
        }

        stage('Build') {
            steps {
                script {
                    bat "mvn clean install"
                    echo 'Build completed successfully.'
                }
            }
        }
        
        stage('Test') {
            steps {
                script {
                    bat "mvn test"
                    echo 'Tests executed successfully.'
                }
            }
        }

        stage('Deploy') {
            steps {
                script {
                    // Get the Jenkins workspace path dynamically
                    def baseDir = env.WORKSPACE
                    def artifactPath = "${baseDir}\\target\\crud-app-1.0-SNAPSHOT.jar"
                    def deployDir = "D:\\Deployed"

                    try {
                        bat "if not exist \"${deployDir}\" mkdir \"${deployDir}\""
                        bat "copy \"${artifactPath}\" \"${deployDir}\""
                        echo 'Deployment completed successfully.'
                    } catch (Exception e) {
                        echo "Deployment failed: ${e.message}"
                    }
                }
            }
        }
    }

    post {
        always {
            script {
                echo 'Cleaning up workspace post-build.'
                node {
                    try {
                        deleteDir()
                    } catch (Exception e) {
                        echo "Failed to delete workspace: ${e.message}"
                    }
                }
            }
        }
        success {
            echo 'Build, test, and deploy stages completed successfully.'
        }
        failure {
            echo 'Build, test, or deploy failed. Check logs for details.'
        }
    }
}
