pipeline {
    agent {
        label'master'
    }
    parameters {
        choice(name: 'target_environment', choices: ['qa', 'dev', 'stage'], description: 'cft target Environment',)
        choice(name:'state', choices: ['present', 'absent'], description: 'cft build or teardown condition',)
        string(name: 'stack_name', defaultValue: '', description: 'cft stackname',)
    }
    stages {
         stage('scm checkout') {
            steps {
                git branch: 'master',
                    credentialsId: 'github_id',
                        url: 'https://github.com/oscarose/demo-lab.git'
            }
         }
         stage('deploy qa stack') {
             when {
                expression { params.target_environment == 'qa' }
             }
             steps {
                 withCredentials([[$class: 'AmazonWebServicesCredentialsBinding', credentialsId: 'aws_id', accessKeyVariable: 'aws_access_key', secretKeyVariable: 'aws_secret_key']]){
                     sh """
                     ansible-playbook  --extra-vars "ansible_python_interpreter=/bin/python stack_name=${stack_name} state=${state} Environment=${target_environment}" ${WORKSPACE}/jenkins.yaml
                     """
                 }
             }
         }
         stage('deploy dev stack') {
             when {
                expression { params.target_environment == 'dev' }
             }
             steps {
                 withCredentials([[$class: 'AmazonWebServicesCredentialsBinding', credentialsId: 'aws_id', accessKeyVariable: 'aws_access_key', secretKeyVariable: 'aws_secret_key']]){
                     sh """
                     ansible-playbook  --extra-vars "ansible_python_interpreter=/bin/python stack_name=${stack_name} state=${state} Environment=${target_environment}" ${WORKSPACE}/jenkins.yaml
                     """
                 }
             }
         }
    }
}
