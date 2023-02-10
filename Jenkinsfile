pipeline {
	agent {
		label {
			label 'built-in'
		}
	}
	
	stages {
		stage ('clone-compose'){
			dir ('/mnt/git-repo'){
				sh "rm -rf compose"
				sh "git clone https://github.com/Pramod-Khutwad/compose.git"
			}
		}
		
		stage ('clone-gameoflife'){
			steps {
				dir ('/mnt/projects'){
					sh "rm -rf game-of-life"
					sh "git clone https://github.com/Pramod-Khutwad/game-of-life.git"				
				}
			}
		}
		
		stage ('build-gameoflife'){
            steps {
                sh "mvn -f /mnt/projects/game-of-life/pom.xml install"
            }
		}
		
		stage ('copy-gameoflife.war'){
			steps {
				sh "cp /mnt/projects/game-of-life/gameoflife-web/target/gameoflife.war /mnt/wars"
			}
		}
		
		stage ('copy-war-to-slave'){
			steps {
				sh "scp -i mywindowskey.pem /mnt/wars/gameoflife.war ec2-user@172.31.7.29:/mnt/wars"
				sh "scp -i mywindowskey.pem /mnt/compose/docker-compose.yaml ec2-user@172.31.7.29:/mnt/compose"
			}
		}
	
		stage ('copy-docker-compose.yaml'){
			agent {
				label {
					label '172.31.7.29'
				}
			}	
			
			steps {				
				dir ('/mnt/compose'){
					sh "docker-compose up -d"
				}
			}
		}
	}
}
