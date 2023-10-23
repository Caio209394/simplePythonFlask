pipeline {
    agent any

    stages {
        stage('Build') {
            steps {
                sh 'docker build -t simplepythonflash .'
            }
        }
        
        stage ("Executa Teste"){
            steps {
                sh 'docker run -i --rm --name=teste simplepythonflask'
                sh 'sleep 10'
		sh 'docker exec -ti teste nosetests --with-xunit --with-coverage --cover-package=project test_users.py'
		sh 'docker cp teste:/courseCatalog/nosetests.xml .'
		junit 'nosetests.xml'
		
		sh "sonar-scanner \
	  		-Dsonar.projectKey=courseCatalog \
  			-Dsonar.sources=. \
  			-Dsonar.host.url=http://sonarqube:9000 \
		I	-Dsonar.login=sqp=sqp_7006f2fa278f17c8414ab21b8030935cf055981f"
            }
        }
    }
post {
        always {
            echo 'One way or another, I have finished'
            deleteDir() /* clean up our workspace */
        }
        success {
            echo 'Finalizei com sucesso!'
	   sh 'docker stop teste'
        }
        unstable {
            echo 'I am unstable :/'
        }
        failure {
            echo 'Ocorreu uma falha :('
	    sh 'docker stop teste'
        }
        changed {
            echo 'Things were different before...'
        }
    }
}
