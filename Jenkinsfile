pipeline {
    agent any

    stages {

        stage('Checkout Code') {
            steps {
                git 'git@github.com:quzhijing/jenkins-ansible-pipeline.git'
            }
        }

        stage('Shell Practice') {
            steps {
                sh '''
                echo "=== Shell Practice Start ==="
                echo "Current user:"
                whoami

                echo "Current path:"
                pwd

                echo "List files:"
                ls -l

                echo "System info:"
                uname -a

                echo "Time:"
                date

                echo "=== Environment Variables ==="
                echo "HOME: $HOME"
                echo "PATH: $PATH"
                echo "USER: $USER"

                echo "=== File Operations ==="
                echo "Create a test file:"
                echo "Hello Jenkins!" > test_file.txt
                cat test_file.txt
                rm test_file.txt

                echo "=== Network Commands ==="
                echo "Ping localhost:"
                ping -c 2 127.0.0.1 || echo "Ping failed"

                echo "Check network interfaces:"
                ip addr show || ifconfig

                echo "=== SSH Example (assuming SSH key configured) ==="
                echo "SSH to remote host (example, replace with actual host):"
                # ssh -o StrictHostKeyChecking=no user@host "echo 'SSH Success'; uname -a" || echo "SSH failed - configure keys first"

                echo "=== Archive and Compress ==="
                echo "Create and compress a directory:"
                mkdir -p test_dir
                echo "Test content" > test_dir/file1.txt
                tar -czf test_dir.tar.gz test_dir
                ls -l test_dir.tar.gz
                rm -rf test_dir test_dir.tar.gz

                echo "=== Process Management ==="
                echo "List processes:"
                ps aux | head -5

                echo "=== Disk Usage ==="
                df -h

                echo "=== Shell Practice End ==="
                '''
            }
        }

        stage('Run Ansible') {
            steps {
                sh '''
                echo "=== Run Ansible ==="
                ansible-playbook -i inventory/hosts.ini playbook.yml
                '''
            }
        }
    }
}