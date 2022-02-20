node {
	def app
	def image = 'registry.hub.docker.com/lylio/configaro'
	def branch = 'main'

	try {
		stage('Clone repository') {
	    	git branch: branch,
	        	credentialsId: 'Lylio-GitHub',
	        	url: 'https://github.com/Lylio/configaro.git'
	    }

	    stage('Run Unit Tests') {
        	        sh './mvnw test'
        	    }

        stage('SonarQube: Analysis') {
                        withSonarQubeEnv('SonarQube_Server') {
                            sh 'mvn org.sonarsource.scanner.maven:sonar-maven-plugin:3.7.0.1746:sonar'
                    }
                }

        stage("SonarQube: Quality Gate") {
                sleep(time:3,unit:"SECONDS")
                timeout(time: 10, unit: 'MINUTES') {
                waitForQualityGate abortPipeline: true
                }
        }

		stage('Build JAR') {
	    	docker.image('maven:3.6.3-jdk-11').inside('-v /root/.m2:/root/.m2') {
	        	sh 'mvn -B clean package'
	        	stash includes: '**/*.jar', name: 'jar'
	    	}
	    }

	    stage('Build Docker Image') {
	    	unstash 'jar'
			app = docker.build image
	    }

	    stage('Push to DockerHub') {
	    	docker.withRegistry('https://registry.hub.docker.com', 'dockerhub_id') {
				app.push("${env.BUILD_NUMBER}")
				app.push("latest")
	        }
	    }
	} catch (e) {
		echo 'Error occurred during build process!'
		echo e.toString()
		currentBuild.result = 'FAILURE'
	} finally {
        junit '**/target/surefire-reports/TEST-*.xml'
	}
}