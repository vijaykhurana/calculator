pipeline {
	agent any
	stages {
		stage("Compile") {
			steps {
				sh "./gradlew clean compileJava"
			}
		}
		stage("Unit test"){
			steps{
				sh "./gradlew test"
			}
		}
		stage("Code coverage"){
			steps {
				sh "./gradlew jacocoTestReport"
				publishHTML (target: [
					reportDir: 'build/reports/jacoco/test/html',
					reportFiles: 'index.html',
					reportName: "JaCoCo Report"

				])
				sh "./gradlew jacocoTestCoverageVerification"
			}
		}
		stage("Static code analysis") {
			steps{
				sh "./gradlew checkstyleMain"
				publishHTML (target: [
					reportDir: 'build/reports/checkstyle',
					reportFiles: 'main.html',
					reportName: "Checkstyle Report"

				])
			}
		}
		stage("SonarQube analysis"){
		   steps{
			 		script {
				 def scannerHome = tool 'SonarQubeScanner';
	       withSonarQubeEnv('SonarQube') {
		     sh "${scannerHome}/bin/sonar-scanner -Dsonar.projectVersion=1.0 -Dsonar.projectKey=my.proj -Dsonar.sources=."
				 }
				 }

			}
		}
	}
}
