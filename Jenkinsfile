pipeline {
	agent any
	stages {
		stage('Checkout SCM') {
			steps {
				git 'https://github.com/DanialAshidiq/loginTemplate.git'
			}
		}

		stage('OWASP DependencyCheck') {
			steps {
				dependencyCheck additionalArguments: '--format HTML --format XML', odcInstallation: 'Default'
			}
		}
	}
	stage('Code Quality Check via SonarQube') {
		steps {
			script {
			def scannerHome = tool 'SonarQube';
			withSonarQubeEnv('SonarQube') {
			sh "${scannerHome}/bin/sonar-scanner -Dsonar.projectKey=LabTest -Dsonar.sources=. -Dsonar.host.url=http://127.0.0.1:9000 -Dsonar.login=sqp_6ff9a9ac389cb36be9b572129708e14051275ca3"
			}
			}
			}
			}
			}
	post {
		always {
			recordIssues enabledForFailure: true, tool: sonarQube()
		}
		success {
			dependencyCheckPublisher pattern: 'dependency-check-report.xml'
		}
	}
}
