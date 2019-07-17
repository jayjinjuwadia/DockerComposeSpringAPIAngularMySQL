node {

   checkout scm
/* stage('building spring and ang') {
	sh 'mvn -f ./Spring/pom.xml'
	sh '''
	pushd ./Angular/
	ng build --prod
	popd
	'''
    } */
 /* stage('previous pipeline cleanup') {
	sh '''
	sudo docker container stop databasecontainer springcontainer angularcontainer && sudo docker container rm databasecontainer springcontainer angularcontainer && sudo docker volume prune -f && docker network rm frontend backend && docker image rm -f dbimage springimage angimage 
	'''
    } */
    stage('Network Creation') {
	sh 'sudo docker network create backend' 
	sh 'sudo docker network create frontend'
    }
    stage('Database creation') {
	sh 'sudo chown root:jenkins /run/docker.sock'
	def dockerfile1 = 'DatabaseDockerfile'
        def dockerim1 = docker.build("dbimage", "-f ${dockerfile1} .")
	sh '''
	sudo docker container create --name databasecontainer --network backend dbimage
	'''
    }
    stage('Spring creation') {
        def dockerfile2 = 'SpringApiDockerfile'
	def dockerim2 = docker.build("springimage", "-f ${dockerfile2} .")
	sh '''
	sudo docker container create --name springcontainer --network backend springimage
	sudo docker network connect frontend springcontainer
	'''
    }
    stage('Angular creation') {
	def dockerfile3 = 'AngularDockerfile'
	def dockerim3 = docker.build("angimage", "-f ${dockerfile3} .")
	sh '''
	sudo docker container create --name angularcontainer --network frontend angimage
	'''
    }
    stage('Container startup') {
	sh '''
	sudo docker container start databasecontainer springcontainer angularcontainer
	'''
    }
}

