properties([
    parameters([
        string(defaultValue: 'Installation', name: 'Playbook Name'),
        choice(choices: ['Dry-Run','Playbook-deploy'], name: 'Playbook Action')
    ])
])
pipeline {
    agent any 
    stages {
        stage('Preparing') {
            steps{
                sh 'echo Preparing'
            }
        }
        stage('Git Pulling') {
            steps{
                git branch: 'project', url: 'https://github.com/adahmo/Jenkins-CICD-Ansible.git'
            }
        }
        stage('Playbook Initializing') {
            steps{
                sh 'echo Playbook Initializing'
            }
        }
        stage('Playbook Running') {
            when {
                expression { params['Playbook Action'] == 'Dry-Run' || params['Playbook Action'] == 'Playbook-deploy' }
            }
            steps {
                script {
                    if (params['Playbook Action'] == 'Dry-Run') {
                        sh "ansible-playbook --check -i /etc/ansible/hosts --private-key ${credentials('ansible')} ${params["Playbook Name"]}.yml"
                    } else if (params['Playbook Action'] == 'Playbook-deploy') {
                        ansiblePlaybook become: true, credentialsId: 'ansible', disableHostKeyChecking: true, inventory: '/etc/ansible/hosts', playbook: 'Nginx-Uninstallation.yml', vaultTmpPath: ''                   }
                }
            }
        }
        stage('Playbook deployed') {
            steps{
                sh 'echo Deployment done!!!!'
            }
        }
    }
}
