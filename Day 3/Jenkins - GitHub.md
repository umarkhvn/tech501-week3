- [How to Integrate Jenkins into GitHub](#how-to-integrate-jenkins-into-github)
    - [Continuous Integration (CI)](#continuous-integration-ci)
    - [Continuous Deployment (CD)](#continuous-deployment-cd)
    - [Differences](#differences)
      - [Pros:](#pros)
      - [Cons:](#cons)
    - [CI/CD Benefits](#cicd-benefits)


# How to Integrate Jenkins into GitHub

### Continuous Integration (CI)
* Requires Developers to frequently share their work/code into a repository accessible to all relevant parties. This allows others to seamlessly collaborate on their work and to identify any mistakes early.

### Continuous Deployment (CD)
* Refers to the automatic deployment of changes within code to testing and then production environments. This allows for software to always be in a deployable state for production.  

### Differences

The key differences between CI & CD are that CI primarily involves integrating and testing changes multiple times daily to ensure changes in the codebase are merged smoothly with no errors, whereas CD focuses on ensuring the code is always in a deployable state for production by automating the delivery process.

#### Pros:
* Faster Development Cycles
* Improved quality of code
* Frequent collaboration across Multi-Disciplinary Teams

#### Cons:
* Heavy resource investment
* Frequent updates/deployment may overwhelm teams and users
* Robust testing and monitoring can be difficult to implement

### CI/CD Benefits
* Reduces manual workload by automating builds, tests, and deployment cycles
* Increased Business Value with faster delivery. 
* Less risk via human error through automation 
* Small, frequent changes in the codebase 

![Jenkins CI/CD Pipeline for Sparta Test App](jenkins33.png)

For this pipeline, we will be using CD.

Keygen naming convention:

*  ssh-keygen -t rsa -b 4096 -C "ukhan3211@gmail.com"

