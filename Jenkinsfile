node {
    checkout scm
    stage('Network Creation') {
	sh 'docker network create backend' 
	sh 'docker network create frontend'
    }
    stage('Database creation') {
	def dockerfile1 = 'DatabaseDockerfile'
        def dockerim1 = docker.build("dbimage", "-f ${dockerfile1} .")
	sh '''
	docker container create --name databasecontainer --network backend dbimage
	'''
    }
    stage('Spring creation') {
        def dockerfile2 = 'SpringApiDockerfile'
	def dockerim2 = docker.build("springimage", "-f ${dockerfile2} .")
	sh '''
	docker container create --name springcontainer --network backend springimage
	docker network connect frontend springcontainer
	'''
    }
    stage('Angular creation') {
	def dockerfile3 = 'AngularDockerfile'
	def dockerim3 = docker.build("angimage", "-f ${dockerfile3} .")
	sh '''
	docker container create --name angularcontainer --network frontend angimage
	'''
    }
    stage('Container startup') {
	sh '''
	docker container run databasecontainer springcontainer angularcontainer
	'''
    }
}

