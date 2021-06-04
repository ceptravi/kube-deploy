@Library('ssh_deploy') _

node {
  checkout scm
  sshDeploy('deployment.yml');
}