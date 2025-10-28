🛠️ Jenkins + SonarQube Quality Gate Troubleshooting Guide
Issue: Jenkins build stalls at waitForQualityGate() after SonarQube analysis.

Root Cause: Jenkins awaits a webhook from SonarQube to confirm analysis completion and quality gate status. If the webhook is missing or misconfigured, the pipeline hangs.

Fix Steps:

✅ Configure Webhook in SonarQube

Go to Administration → Webhooks

Add:
Name: Jenkins Quality Gate  
URL: http://<jenkins-host>:8080/sonarqube-webhook/

✅ Ensure Jenkins Is Reachable

Open port 8080 in EC2 security group

Use public/private IP based on VPC setup

✅ Verify Jenkins Credentials

Store SonarQube token in Jenkins as Secret Text

Use credentialsId: 'sonar-token' in pipeline

✅ Add Timeout to Avoid Hanging
timeout(time: 3, unit: 'MINUTES') {
  waitForQualityGate abortPipeline: true, credentialsId: 'sonar-token'
}

✅ Check Webhook Delivery Logs

Visit SonarQube → Admin → Webhooks

Confirm delivery status and error messages


## 🛠️ Jenkins Pipeline Stall on EC2 – Recovery Notes

**Issue:** Jenkins build stalled during SonarQube integration; EC2 instance became unresponsive.

**Diagnosis:**
- Verified low CPU and memory usage via `top` (99% idle, 18% RAM used).
- Detected missing swap space and potential I/O stall.
- SonarQube token generation failed due to unreachable port 9000.

**Resolution:**
- Added 2GB swap to stabilize JVM memory behavior.
- Restarted Jenkins safely via `systemctl`.
- Validated SonarQube status via `/api/system/status`.
- Identified firewall block on port 9000; updated security group to allow inbound traffic.

**Next Steps:**
- Migrate Jenkins workspace to EBS volume.
- Implement build throttling and webhook timeout alerts.
- Document token-based SonarQube auth for future CI/CD resilience.



🐳 Docker Build Fails in Jenkins: permission denied on Docker socket
❌ Problem
During the docker build stage in Jenkins, the following error appeared:

Code
Got permission denied while trying to connect to the Docker daemon socket at unix:///var/run/docker.sock
This prevented Jenkins from building or pushing Docker images.

🧠 Root Cause
The Jenkins user did not have permission to access the Docker daemon socket (/var/run/docker.sock). This is required for running Docker CLI commands from Jenkins.

✅ Resolution
Add Jenkins user to the docker group:

bash
sudo usermod -aG docker jenkins
Restart Docker and Jenkins:

bash
sudo systemctl restart docker
sudo systemctl restart jenkins
Verify access:

bash
sudo su - jenkins
docker info
Ensure the Jenkins user can run Docker commands without errors.

Re-run the Jenkins job to confirm the fix.

