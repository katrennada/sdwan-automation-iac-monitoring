# SD-WAN Automation & Monitoring Lab
End-to-end automation lab using **Terraform**, **Ansible**, **Jenkins**, and **Prometheus/Grafana** to simulate a multi-site SD-WAN deployment and monitoring stack.

## What this lab covers
- Provision VMs on AWS Free Tier (EC2)
- Configuring network services using Ansible (VPN, routes, DNS)
- CI/CD pipeline with Jenkins to trigger Ansible plays
- Real-time monitoring with Prometheus & Grafana dashboards
- Python-based network connectivity testing

## Lab Topology
    +---------+        VPN        +---------+
    | Site A  | <===============> | Site B  |
    +---------+                  +---------+
         \                           /
          \                         /
          +--------+     +--------+
          | Jenkins| --> | Prometheus + Grafana |
          +--------+     +--------+

## Tools Used

| Tool       | Role                               |
|------------|------------------------------------|
| Terraform  | VM provisioning (AWS EC2)     |
| Ansible    | Network config automation          |
| Jenkins    | CI/CD for config deployment        |
| Docker     | Running Prometheus + Grafana stack |
| Prometheus | Network metrics collection         |
| Grafana    | Visualization of metrics           |
| Python     | Network tests (ping, HTTP, ports)  |

---

## How to Use (Quick Overview)

1. **Terraform**: Provision EC2 VMs on AWS (Free Tier)
2. **Ansible**: Configure VPN, routing, DNS
3. **Jenkins**: Trigger automation on change
4. **Monitoring**: Visualize metrics with Grafana
5. **Tests**: Validate service availability

---

## Sample Jenkins Pipeline

```groovy
pipeline {
  agent any
  stages {
    stage('Deploy Infra') {
      steps {
        sh 'cd terraform && terraform apply -auto-approve'
      }
    }
    stage('Configure Network') {
      steps {
        sh 'ansible-playbook -i ansible/inventory.ini ansible/siteA.yml'
      }
    }
    stage('Run Tests') {
      steps {
        sh 'python3 tests/test_ping.py'
      }
    }
  }
}

