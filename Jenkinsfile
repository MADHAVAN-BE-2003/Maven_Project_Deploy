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
                        userRemoteConfigs: [[url: 'https://github.com/MADHAVAN-BE-2003/Maven_Project_1.git']],
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
            echo 'Build and test stages completed successfully.'
        }
        failure {
            echo 'Build or Test failed. Check logs for details.'
        }
    }
}
