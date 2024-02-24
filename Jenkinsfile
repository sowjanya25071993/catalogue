pipeline{
    agent{
        node{
            label 'Agent-1'
        }
    }
    environment{
        PackageVersion=''
    }
    options{
        timeout(time: 1, unit: 'HOURS')
        disableConcurrentBuilds()
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