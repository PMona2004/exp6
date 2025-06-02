exp 6
sudo update-alternatives --config java

Select JDK 8
mvn archetype:generate -DgroupId=com.example -DartifactId=hello-maven -
DarchetypeArtifactId=maven-archetype-quickstart -DinteractiveMode=false
cd hello-maven
// UPDATE POM file if needed
mvn package // to check if maven project works in local system
mvn clean
// delete the package files

Change JDK version ( 17, 21 )
sudo apt update
1. sudo apt install openjdk-21-jdk -y
2. sudo wget -O /usr/share/keyrings/jenkins-keyring.asc https://pkg.jenkins.io/debian-stable/jenkins.io-2023.key
3. echo "deb [signed-by=/usr/share/keyrings/jenkins-keyring.asc] https://pkg.jenkins.io/debian-stable binary/" | sudo tee /etc/apt/sources.list.d/jenkins.list > /dev/null
5. sudo apt update
6. sudo apt install jenkins -y
7. sudo systemctl status jenkins
   (sudo systemctl stop,restart,start jenkins)
 

get- sudo cat /var/lib/jenkins/secrets/initialAdminPassword

# Install Git
sudo apt update &amp;&amp; sudo apt install git -y
git --version

# Configure Git (Replace with your details)
git config --global user.name &quot;Your Name&quot;
git config --global user.email &quot;your-email@example.com&quot;
git config --global init.defaultBranch main
# Generate SSH Key (Press Enter for defaults)
ssh-keygen -t rsa -b 4096 -C &quot;your-email@example.com&quot; -f ~/.ssh/id_rsa -N &quot;&quot;
# Start SSH agent &amp; add key
eval &quot;$(ssh-agent -s)&quot;
ssh-add ~/.ssh/id_rsa
# Print SSH key (Copy this manually and add to GitHub SSH keys)
cat ~/.ssh/id_rsa.pub
# Pause to allow you to add the key on GitHub
read -p &quot;Go to GitHub &gt; Settings &gt; SSH Keys, add the copied key, then press Enter 
# Test SSH connection to GitHub
ssh -T git@github.com

// Create a new repository on github using CURL, you will have to generate tokens by
going to Settings - Developor settings - Tokens classic -  Generate new token

echo "# hello-maven" >> README.md # Create README file
git init # Initialize Git repo
git add . # Stage ALL files
git commit -m &quot;first commit&quot; # Commit everything
git branch -M main # Rename branch to &#39;main&#39;
git remote add origin git@github.com:azadCMRIT/hello-maven.git # Add GitHub remote
git push -u origin main # Push to GitHub


pipeline {
 agent any
 stages {
     stage('Checkout') {
         steps {
             git branch: 'main', url:"https://github.com/PMona2004/Mymaven.git"
             }
      }
     stage("Build") {
         steps {
             sh "mvn clean package"
             }
     }
     stage("Test") {
         steps {
             sh "mvn test"
             }
      }
 }
} 
exp 7

hosts.ini
[local]
localhost ansible_connection=local

setup.yaml
---
- name: Basic Server Setup
hosts: localhost
become: yes 

tasks:
- name: Update apt cache
apt:
update_cache: yes

- name: Install curl
apt:
name: curl
state: present

ansible-playbook -i hosts.ini setup.yaml --
ask-become-pass
if password is asked, type “root” without double quotes

exp 8

Go to /var/lib/jenkins/workspace/ :
sudo nano deploy.yaml
---
- name: Deploy Artifact to Localhost
hosts: localhost
become: true
become_user: student-devops //exam?
become_method: su
tasks:
- name: Copy the artifact to the target location
copy:
src: "/var/lib/jenkins/workspace/maven/target/hello-maven-1.0-SNAPSHOT.jar"
dest: "/home/student-devops/Desktop/t.jar"

sudo nano hosts.ini
Put the following contents into it :
[local]
localhost ansible_connection=local

Add the following script in pipeline script :

pipeline {
    agent any

    stages {
        stage('Checkout') {
            steps {
                git branch: 'main', url: 'git@github.com:PMona2004/hello-maven.git'
            }
        }

        stage('Build') {
            steps {
                sh 'mvn clean package'
            }
        }

        stage('Test') {
            steps {
                sh 'mvn test'
            }
        }

        stage('Archive Artifacts') {
            steps {
                archiveArtifacts artifacts: '**/target/*.jar', allowEmptyArchive: true
            }
        }

        stage('Deploy') {
            steps {
                sh '''
                    export ANSIBLE_HOST_KEY_CHECKING=False
                    ansible-playbook -i /var/lib/jenkins/workspace/maven2/hosts.ini \
                    /var/lib/jenkins/workspace/maven2/deploy.yaml \
                    --extra-vars='ansible_become_pass=login@123'
                '''
                // Enter student password above (login@123)
            }
        }
    }
}

exp 11,12 releaeses artifact + , Stages + empty job give name THEN 
Go to “pre deployment conditions” by clicking on Stage 
Select Manual only
13. Now “save” the release pipeline from top right corner  .  Create folder
create release (top right) select version  - create

now releases (under pipeline) will have release 1 
Go to deploy (Deploy drop down on top)
Select Stage and go to “Deploy stage”
Select “Deploy “ and wait for sometime





