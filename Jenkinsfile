
properties([pipelineTriggers([githubPush()])])

node {

checkout([$class: 'GitSCM', branches: [[name: '*/master']], extensions: [], userRemoteConfigs: [[credentialsId: 'e77b9a03-e38f-4419-a40b-8b164bd80bbb', url: 'https://github.com/netdevops-1/dc-automation.git/']]])

stage('Build') {
    echo 'Build stage'
    sh "docker run -dit --name ansible_git netdevops/ansible_git_v1"
    sh 'docker exec -i ansible_git /bin/sh -c "git clone https://github.com/netdevops-1/dc-automation.git"'
}
stage('Test') {
    echo 'Test stage'
    sh 'docker exec -i ansible_git /bin/sh -c "ansible-playbook dc-automation/vlan.yml -i dc-automation/hosts --syntax-check"'
    sh 'docker exec -i ansible_git /bin/sh -c "ansible-playbook dc-automation/vlan.yml -i dc-automation/hosts --check"'
}        
stage('Clean up') {
    echo 'Clean up stage'
    sh "docker rm ansible_git -f"
}
}
