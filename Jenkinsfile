pipeline {
	agent any
	triggers {
		 pollSCM('* * * * *')
	 }
	stages {
		stage('Cleanup') {
			steps {
				echo 'Removing old configuration...'
                                sh 'rm -rf /var/lib/jenkins/workspace/ansible-mysql/*.retry'
			}
		}
		stage('Check') {
			steps {
				echo 'Modules Validating...'
                                sh 'cd /var/lib/jenkins/workspace/ansible-mysql/ && ansible-playbook mysql_playb.yml -i hosts --check'
			}
		}
		stage ('Apply'){
			steps{
				echo 'Applying changes ....'
				timeout(time:5, unit:'DAYS'){
					input message:'Do you want apply this changes?'
				}
                                sh 'cd /var/lib/jenkins/workspace/ansible-mysql/ && ansible-playbook mysql_playb.yml -i hosts'
			}
			post {
				success {
					echo 'Changes applied successfully.'
				}
				failure {
					echo ' Something failed, Check logs.'
				}
			}
		}
	}
}
