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
                sh 'env'
                sh 'ls -l'
                sh 'pwd'
              }
          }
        }
        
        stage('Preparing zLinux') {
          steps {
              node ('tvt1032.svl.ibm.com'){
                sh "echo ${params.zoweBuild}"
                sh 'echo $HOSTNAME'
                sh 'env'
                sh 'ls -l'
                sh 'pwd'
                sh '/tmp/jcl/daily_build/dailyBuild.sh
            }
          }
        }
        
        
        stage('install build') {
          steps {
              node ('winmvs3b.hursley.ibm.com'){
                sh 'env'
                sh 'ls -l'
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
