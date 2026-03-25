pipeline {
    agent any

    parameters {
        string(name: 'TARGET_HOST', defaultValue: '192.168.0.200', description: '目标主机 IP 地址')
        string(name: 'ANSIBLE_USER', defaultValue: 'jqu003', description: 'Ansible 连接用户名')
        booleanParam(name: 'UPDATE_SYSTEM', defaultValue: false, description: '是否更新系统（Update apt cache / Upgrade）')
        booleanParam(name: 'INSTALL_MINICOM', defaultValue: false, description: '是否安装 minicom')
        booleanParam(name: 'INSTALL_COMMON', defaultValue: true, description: '是否安装常用工具（vim, git, curl, wget, net-tools, htop）')
    }

    stages {

        stage('Checkout Code') {
            steps {
                checkout scm
            }
        }

        stage('Validate Ansible Playbook') {
            steps {
                sh '''
                echo "=== Checking Ansible Installation ==="
                ansible --version
                
                echo ""
                echo "=== Validating Playbook Syntax ==="
                ansible-playbook playbook.yml -i inventory/hosts.ini --syntax-check
                
                echo ""
                echo "=== Linting Playbook (optional) ==="
                ansible-lint playbook.yml || echo "Warning: ansible-lint found issues"
                '''
            }
        }

        stage('Prepare Variables') {
            steps {
                script {
                    echo "=== Build Parameters ==="
                    echo "TARGET_HOST: ${params.TARGET_HOST}"
                    echo "ANSIBLE_USER: ${params.ANSIBLE_USER}"
                    echo "UPDATE_SYSTEM: ${params.UPDATE_SYSTEM}"
                    echo "INSTALL_MINICOM: ${params.INSTALL_MINICOM}"
                    echo "INSTALL_COMMON: ${params.INSTALL_COMMON}"
                }
            }
        }

        stage('Run Ansible Playbook') {
            steps {
                sshagent(['e8e39288-3317-4b60-a00b-438c9dc73e4d']) {
                    sh '''
                    echo "=== Starting Ansible Deployment ==="
                    
                    ansible-playbook playbook.yml \
                      -i "${TARGET_HOST}," \
                      -u "${ANSIBLE_USER}" \
                      --ssh-common-args='-o StrictHostKeyChecking=no' \
                      -e "update_system=${UPDATE_SYSTEM}" \
                      -e "install_minicom=${INSTALL_MINICOM}" \
                      -e "install_common=${INSTALL_COMMON}" \
                      -v
                    
                    echo ""
                    echo "=== Ansible Deployment Completed ==="
                    '''
                }
            }
        }
    }

    post {
        success {
            echo '✅ Pipeline executed successfully!'
        }
        failure {
            echo '❌ Pipeline failed. Check logs above.'
        }
        always {
            echo '=== Pipeline Completed ==='
        }
    }
}