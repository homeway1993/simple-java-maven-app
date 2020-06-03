pipeline {
    agent {
        docker {
            image 'maven:3-alpine'
            args '-v /root/.m2:/root/.m2 \
              --env MAVEN_OPTS="-DproxyHost=192.168.201.201 -DproxyPort=3128"'
        }
    }
    stages {
        stage('Checkout') {
            steps {
                checkout([
                    $class: 'SubversionSCM',
                    additionalCredentials: [],
                    excludedCommitMessages: '',
                    excludedRegions: '',
                    excludedRevprop: '',
                    excludedUsers: '',
                    filterChangelog: false,
                    ignoreDirPropChanges: false,
                    includedRegions: '',
                    locations: [
                      [
                        remote: 'https://svn.chowsangsang.com/repos/uat/oc_storefront/microservice/oc-shopping-list/develop/omni-channel-phase1',
                        cancelProcessOnExternalsFail: true,
                        credentialsId: '2babdd56-e2f1-4f9b-b5fc-e4b73bb77ddf',
                        depthOption: 'infinity',
                        ignoreExternalsOption: true,
                        local: 'microservice/oc-shopping-list'
                      ]
                    ]
                ])
            }
        }
        stage('Build') { 
            steps {
                dir('microservice/oc-shopping-list') {
                    sh 'mvn -B -DskipTests clean package'
                }
            }
        }
        stage('Test') {
            steps {
                dir('microservice/oc-shopping-list') {
                    sh 'mvn test'
                }
            }
            post {
                always {
                    junit 'microservice/oc-shopping-list/target/surefire-reports/*.xml'
                }
            }
        }
    }
}