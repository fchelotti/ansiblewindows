pipeline {
    agent {
        label "sul-ansible"
    }

    parameters {
        string(defaultValue: "10.104.244.13", name: "server")
       
    }   

    stages {
        stage('Clone Ansible Repo') {
            steps {
                container('ansible') {
                    git credentialsId: 'bitbucket_token', url: 'https://github.com/fchelotti/ansiblewindows.git', branch: 'windows_ac'
                    //feature/ansible_windows
                }
            }
        }

        stage('Pre Build') {
            steps {
                container('ansible') {
                    sh 'sed -i "/windows]/a ${server}" modulos/hosts'
                }
            }
        }

        stage('PARCHADOS') {
            steps {
                withCredentials([string(credentialsId: 'root_password', variable: 'root_passwd'),
                                 string(credentialsId: 'jenkins_admin_passwd', variable: 'app_passwd')
                ]) {
                container('ansible') {
                    sh '''ansible-playbook -i modulos/hosts modulos/ansible_windows_sul.yaml -e "countries=Brasil" -e "root_password=${root_passwd}" -e "app_password=${app_passwd}" -e "nicdata=${vlanID}" -e "nicmgmt=${vlanMGMT}" -e "nicback=${vlanBKP}" -vvv'''
                    // -e "domain=${domainResult}" -e "organization_unit=\'${strOganizationUnit}\'" -vvv'''
                    }
                }
            }

        }    
    }
}