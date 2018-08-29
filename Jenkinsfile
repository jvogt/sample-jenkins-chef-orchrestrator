pipeline {
    agent any
    environment {
        cookbooks = powershell(returnStdout: true, script: 'knife cookbook list').trim()
        environments = powershell(returnStdout: true, script: 'knife environment list').trim()
    }
    stages {
        stage('Choose Parameters') {
            steps {
                script {
                    env.cookbook = input message: 'Choose a cookbook', ok: 'Choose',
                        parameters: [
                            choice(name: 'Cookbook', choices: env.cookbooks)
                        ]
                    env.environment = input message: 'Choose an environment', ok: 'Choose',
                        parameters: [
                            choice(name: 'Environment', choices: env.environments)
                        ]
                }
            }
        }
        stage('Promote Cookbook') {
            steps {
                powershell "knife cookbook download ${env.cookbook} -N -d c:\users\chef\cookbooks -f"
                powershell "knife spork promote ${env.environment} ${env.cookbook} --remote"
            }
        }
        stage('Run Chef') {
            steps {
                powershell "knife ssh 'chef_environment:${env.environment}' sudo chef-client"
            }
        }
    }
}

