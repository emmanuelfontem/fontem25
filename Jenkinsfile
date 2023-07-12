pipeline {
    agent any
    stages {
        stage('compile') {
	   steps {
                echo 'compiling..'
		git url: 'https://github.com/lerndevops/samplejavaapp'
		sh script: '/opt/maven/bin/mvn compile'
           }
        }
        stage('codereview-pmd') {
	   steps {
                echo 'codereview..'
		sh script: '/opt/maven/bin/mvn -P metrics pmd:pmd'
           }
	   post {
               success {
		   recordIssues enabledForFailure: true, tool: pmdParser(pattern: '**/target/pmd.xml')
               }
           }		
        }
        stage('unit-test') {
	   steps {
                echo 'unittest..'
	        sh script: '/opt/maven/bin/mvn test'
                 }
	   post {
               success {
                   junit 'target/surefire-reports/*.xml'
               }
           }			
        }
  stage('codcoverage') {
	   steps {
                echo 'codcoverage..'
		sh script: '/opt/maven/bin/mvn verify'
           }
	   post {
               success {
		   jacoco maximumBranchCoverage: '10', maximumComplexityCoverage: '10', maximumInstructionCoverage: '10', minimumBranchCoverage: '10', minimumComplexityCoverage: '10', minimumInstructionCoverage: '10', runAlways: true
               }
           }		
        }      


stage('package') {
	   steps {
                echo 'package......'
		sh script: '/opt/maven/bin/mvn package'	
           }		
        }
    }
}
