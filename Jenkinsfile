pipeline {
    agent any
    
	// should be triggered every 10 minutes on Thursdays
    triggers {
        cron('H/10 * * * 4')
    }

    tools {
        maven 'MAVEN3'
    }

    stages {

    	stage('Checkout') {
            steps {
				git branch: 'main', url: 'https://github.com/shield0225/spring-petclinic.git'
            	}
        	}

        stage('Build') {
            steps {
                bat 'mvn clean package'
            }
        }

		// include one stage which using Jacoco generates code coverage report
        stage('Test with JaCoCo') {
            steps {
                tool 'Maven'
                bat 'mvn test jacoco:report'
            }

            post {
                always {
                    jacoco execPattern: '**/target/jacoco.exec', classPattern: '**/classes', sourcePattern: '**/src/main/java'
                }
            }
        }
    }

    post {
        success {
            echo 'Build was successful!'
        }

        failure {
            echo 'Build failed.'
        }
    }
}
