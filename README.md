## EC2 Deployment via Terraform + Jenkins (Auto Scaling Group with user_data)

This project demonstrates how to automatically deploy an EC2 instance using Terraform through a Jenkins pipeline. The EC2 is provisioned via a Launch Template and Auto Scaling Group (ASG) and deleted automatically after testing

---

### What is being tested/proved?
- Automating infrastructure provisioning via Jenkins
- Terraform deployment of EC2 with ASG and user_data
- Automated clean-up (terraform destroy)

---

### Tools & Technologies
- Terraform
- AWS (VPC, Subnet, ASG, Launch Template, EC2)
- Jenkins (Declarative Pipeline)

---

### Success Criteria
- Jenkins applies Terraform successfully.
- EC2 launches with Apache installed and a custom index page.
- Infrastructure is cleaned up (destroyed) at the end.

---

### File Structure
```
├── main.tf          # Terraform code to deploy infrastructure
├── Jenkinsfile      # Jenkins pipeline to automate apply & destroy
├── README.md        # This documentation
├── console_output.txt # Jenkins console output (attached post run)
```

---

### How to Reproduce

#### 1. Clone this repo
```bash
git clone <REPO_URL>
cd <REPO_NAME>
```

#### 2. Create Jenkins Pipeline Job
- Go to Jenkins > New Item > Pipeline
- Under **Pipeline Script**, use the `Jenkinsfile` from this repo.
- Add AWS credentials (e.g., via environment variables or credentials plugin).

#### 3. Run the Jenkins Job
- Click **Build Now**.
- Observe output in **Console Output**.

#### 4. Expected Output
- An EC2 instance created with Apache installed.
- A message at `/var/www/html/index.html`.
- After 2 minutes, the instance is destroyed.

---

### Auto-Cleanup
- The pipeline waits for 2 minutes and destroys the resources.
- This helps avoid unnecessary AWS charges.

---

### Jenkins Output
Refer to `console_output.txt` for full output from Jenkins.

---
