Important URL's
https://www.jenkins.io/doc/book/pipeline/jenkinsfile/

Installation on controller
sudo apt update
sudo apt install fontconfig openjdk-17-jre
java -version

sudo wget -O /usr/share/keyrings/jenkins-keyring.asc https://pkg.jenkins.io/debian-stable/jenkins.io-2023.key
echo "deb [signed-by=/usr/share/keyrings/jenkins-keyring.asc]" https://pkg.jenkins.io/debian-stable binary/ | sudo tee /etc/apt/sources.list.d/jenkins.list > /dev/null
sudo apt-get update
sudo apt-get install jenkins

Installation on slave
sudo apt update
sudo apt install fontconfig openjdk-17-jre

sudo usermod -aG sudo jenkins
sudo usermod -aG docker jenkins

Configuring Webhook for github.
Copy the jenkins url up to :8080
Go to GitHub repository. Click on Settings > Webhook > Add webhook and add below.
In payload url add <jenkins url copied above up to 8080>/github-webhook/.
Select Content type as application/json. Click on Add webhook.

Configuring Webhook for sonarqube.
Login to sonarqube console.
Administration > Configuration > Webhooks > Create
Copy the jenkins url uo to :8080. In url section in popup paste copied url along with /sonarqube-webhook/. Entire url looks like below
http://<jenkins_url>:8080/sonarqube-webhook/

Installing Maven
sudo apt-get install -y maven

Installing Trivy
sudo apt-get install wget apt-transport-https gnupg lsb-release
wget -qO - https://aquasecurity.github.io/trivy-repo/deb/public.key | sudo apt-key add -
echo deb https://aquasecurity.github.io/trivy-repo/deb $(lsb_release -sc) main | sudo tee -a /etc/apt/sources.list.d/trivy.list
sudo apt-get update
sudo apt-get install trivy

or 

wget https://github.com/aquasecurity/trivy/releases/download/v0.18.3/trivy_0.18.3_Linux-64bit.deb
sudo dpkg -i trivy_0.18.3_Linux-64bit.deb

Jfrog
sudo usermod -aG docker $USER
docker pull docker.bintray.io/jfrog/artifactory-oss:latest
sudo mkdir -p /jfrog/artifactory
sudo chown -R 1030 /jfrog/
docker run --name artifactory -d -p 8081:8081 -p 8082:8082 -v /jfrog/artifactory:/var/opt/jfrog/artifactory docker.bintray.io/jfrog/artifactory-oss:latest
default values:
username: admin
password: password

sonarqube:
docker run -d --name sonarqube -p 9000:9000 -p 9092:9092 sonarqube
default values:
username: admin
password:admin

Environmental Variables
https://www.jenkins.io/doc/book/pipeline/jenkinsfile/#using-environment-variables

Plugins
Parameterized Trigger
Sonar Scanner
Artifactory
JFrog
