pipeline {
    agent any
    tools {
        jdk 'jdk8'
        maven 'maven3'
    }
    stages {
        stage('install and sonar parallel') {
            steps {
                parallel(install: {
			sh "mvn -U clean test cobertura:cobertura -Dcobertura.report.format=xml"
                    }, sonar: {
			sh "mvn sonar:sonar" 
                    })
            }
            post {
                always {
                    junit '**/target/*-reports/TEST-*.xml'
                    step([$class: 'CoberturaPublisher', coberturaReportFile: 'target/site/cobertura/coverage.xml'])
                }
            }
        }
	stage ('deploy'){
            steps{
                configFileProvider([configFile(fileId: '49a1c083-fc08-4126-99e8-c25b786e72c6', variable: 'SETTINGS')]) {
                    sh "mvn -s $SETTINGS deploy -DskipTests"
                }
            }
        }
    }
}
