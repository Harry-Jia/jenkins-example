pipeline {
    agent any
    
    parameters {
        string(defaultValue: 'Text testing', description: '', name: 'StringDemo')
    }

    stages {
        
        stage('Preparing') {
          steps {
              node ('smuVM001'){
                sh "echo ${params.zoweBuild}"
                sh 'echo $HOSTNAME'
              }
          }
        }
        
        stage('Downloading build') {
          steps {
              node ('master'){
                sh 'echo $HOSTNAME'
                sh "echo ${params.zoweBuild}"
                sh "echo ${params.buildDate}"
                sh 'pwd'
                sh "/root/download_pax_build.sh ${params.zoweBuild} ${params.buildDate}"
                sh 'ls -l'
              }
          }
        }
        
        stage('install build') {
          steps {
              node ('sp14.svl.ibm.com'){
                sh 'echo $HOSTNAME'
                sh "echo ${params.zoweBuild}"
                sh "echo ${params.buildDate}"
                sh 'pwd'
                sh ".zowe-0.9.4/install/zowe-pre-install.sh"
                sh 'ls -l'
                sh 'env'
              }
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
