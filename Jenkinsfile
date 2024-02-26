pipeline{
    agent{
        node{
            label 'Agent-1'
        }
    }
    environment{
        PackageVersion=''
        nexusURL= '172.31.2.247:8081'
    }
    options{
        timeout(time: 1, unit: 'HOURS')
        disableConcurrentBuilds()
        ansiColor('xterm')
    }
    stages{
        stage('get the version'){
            steps{
                script{
                    def packagejson = readJSON file: 'package.json'
                    PackageVersion = packagejson.version
                    echo "application version:$PackageVersion"
                }
            }
        }
        stage('install dependancies'){
            steps{
                sh """
                  npm install
                """
            }
        }
        stage('build'){
            steps{
                sh """
                  ls -la
                  zip -q -r catalogue.zip ./* -x ".git" -x "*.zip"
                  ls -ltr
                """
            }
        }
        stage('publish artifact'){
            steps{
              nexusArtifactUploader(
                nexusVersion: 'nexus3',
                protocol: 'http',
                nexusUrl: "${nexusURL}",
                groupId: 'com.roboshop',
                version: "${PackageVersion}",
                repository: 'catalogue',
                credentialsId: 'nexus-auth',
                artifacts: [
                 [artifactId: 'catalogue',
                  classifier: '',
                  file: 'catalogue.zip',
                  type: 'zip']
                ]
             )
            }
        }
        stage('deploy'){
            steps{
                script{
                    def params = [
                        string(name: 'version', value: "$PackageVersion"),
                        string(name: 'environment', value: "dev")
                        ]
                    build job: "catalogue-deploy", wait:true, parameters: params
                }
            }
        }
    }
    post{
        always{
            echo "i will always say hello again"
            deleteDir()
        }
        success{
            echo "i will always say hello when pipeline is success"
        }
        failure{
            echo "this is when pipeline failed"
        }
    }
}