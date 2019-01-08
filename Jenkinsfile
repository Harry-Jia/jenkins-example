pipeline {
    agent any
    
    parameters {
        string(defaultValue: 'Text testing', description: '', name: 'StringDemo')
    }

    stages {
        
        stage('Preparing') {
          steps {
            sh "echo ${params.StringDemo}"
          }
        }
        
        stage ('Compile Stage') {

            steps {
                withMaven(maven : 'maven_3_6_0') {
                    sh 'mvn clean compile'
                }
            }
        }

        stage ('Testing Stage') {

            steps {
                withMaven(maven : 'maven_3_6_0') {
                    sh 'mvn test'
                }
            }
        }


        stage ('Deployment Stage') {
            steps {
                withMaven(maven : 'maven_3_6_0') {
                    sh 'mvn deploy'
                }
            }
        }
    }
}
