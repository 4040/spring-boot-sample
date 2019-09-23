def label = "springboot-build-${UUID.randomUUID().toString()}"
podTemplate(
	label: label,
	name: label,
	volumes: [
		hostPathVolume(mountPath: '/var/run/docker.sock', hostPath: '/var/run/docker.sock')
	]
){
	properties([
		[$class: 'BuildDiscarderProperty', strategy: [$class: 'LogRotator', numToKeepStr: '10']],
		disableConcurrentBuilds()
	])
	node(label) {
		container(label) {
			stage('Clone') {
				try {
						//git branch: 'develop', credentialsId: 'git_creds', url: ''
						git url: 'https://github.com/4040/spring-boot-sample.git', credentialsId: 'github-4040', branch: 'master'

					
				} catch (exc) {
					currentBuild.result = "FAILURE"
					throw exc
				} finally {
				}
			}
			
			stage ('Build') {
					try {
						 withMaven(maven:'maven') {

						sh 'mvn clean install'
						}
					} catch (exc) {
						currentBuild.result = "FAILURE"
						throw exc
					} finally {
					}
				
			}
			stage("Docker build") {
    withDockerRegistry([credentialsId: 'acr_cred', url: 
    'https://smcpacr.azurecr.io']) {
    sh "docker build ." 
    }
}
		}
	}
}
