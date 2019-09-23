node {


stage('Checkout') {
	git url: 'https://github.com/4040/spring-boot-sample.git', credentialsId: 'github-4040', branch: 'master'
}

stage('Initialize')   {
	def dockerHome = tool 'docker'
	env.PATH = "${dockerHome}/bin:${env.PATH}"
}

	           

stage('Build') {
	 withMaven(maven:'maven') {
	sh 'mvn clean install'
	def pom = readMavenPom file:'pom.xml'
	print pom.version
	env.version = pom.version
	 }
}
	
	  stage("Docker version") {
            withDockerRegistry() {
            sh "docker version" 
            }
            }


stage("Docker build") {
    withDockerRegistry([credentialsId: 'acr_cred', url: 
    'https://smcpacr.azurecr.io']) {
    sh "docker build --no-cache -f dockerfile ." 
    }
}



}
