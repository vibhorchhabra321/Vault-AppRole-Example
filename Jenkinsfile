import groovy.json.JsonSlurper
def roleID
node {
   stage('Clone Source Code') {
      // Get source code from a github repository
      git (
        url: 'https://github.com/vibhorchhabra321/Vault-AppRole-Example.git',
        branch: "master")

   }
   stage('Get RoleID') {
    withCredentials([string(credentialsId: 'appName', variable: 'RoleName')]) {
      // Get Role ID from Vault
    def response = httpRequest customHeaders: [[name: 'X-Vault-Token', value: '64aa8b29-2f13-613e-429e-2290ff0b62a7']], url: "http://172.22.22.105:8200/v1/auth/approle/role/${RoleName}/role-id"
    def json = new JsonSlurper().parseText(response.content)
    roleID = "${json.data.role_id}"
    }
   }
   stage('Deploy App with Ansible') {
    withCredentials([string(credentialsId: 'appName', variable: 'RoleName')]) {
    ansiblePlaybook colorized: true, extras: "-e RoleName=${RoleName} -e RoleID=${roleID}", inventory: '.hosts', playbook: 'deploy.yml'

    }
   }
}
