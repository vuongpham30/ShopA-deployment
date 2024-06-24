SHOPA

I. Components
    - Jenkins
        cd.shopa.vn
            Plugins: sonarqube
    - Sonarqube
        sonarqube.shopa.vn
    - Argocd
        argocd.shopa.vn
    - K8s
        DEV:     https://kubernetes-dev.shopa.vn
        PROD:    https://kubernetes-dev.shopa.vn
        STAGING: https://kubernetes-dev.shopa.vn
    - Domain
        *.shopa.vn
    - Registry
        harbor.shopa.vn
    - Repository
        https://github.com/vuongpham30/Shopa-deployment.git
        https://github.com/vuongpham30/ShopA.git
    - Tools support
        Locust - Load testing
        Slack  - ArgoCD Notification
        Docker scout, Harbor - security vulnerabilities
        Prometheus/grafana - Monitor cluster, trigger node,....
        Lends - Kubernetes IDE
        EFK - Get logs k8s

II. Setup pipeline and tools support
    1. Setup CI
        a. Create project Shopa on SonarQube, Integrate SonarQube With Jenkins
        b. Create pipeline on jenkins use Jenkinsfile Corresponding environment ( ./jenkins/<env>/Jenkinsfile)
        c. Add credential github, Harbor
        d. Add lack Notification Plugin

    2. Setup CD (./application/setup-cd.txt)
        a. Install argocd cli
        b. Login to argocd
        c. Add context clusters
        d. Connect argocd to github repo
        e. Add new cluster to argocd
        f. Create project
        g. Deploy application for each environment (./application/application_<env>)
        h. ArgoCD Notifications & Slack Integration

III. Monitor system
    1. Setup Prometheus/grafana for each clusters
    2. Setup EFK for each clusters

IV. Workflow
    1.Change source code 
        Developers push code to github
        When developer changes any source code the CI jenkins jobs trigger .
        The CI will perform the  following  steps :
        - Download code from the github repository
        - Buil the code into a Docker image
        - Push the docker image to the Harbor registry

    2. Update deployment config
        Jenkins update application version information to application deployment configuration files stored in the config repo (app-deploy.yaml)
        The argoCD will perform the  following  steps:
        - ArgoCD will watch the repository for changes.
            Staging & dev
             - When a change detected, ArgoCD will  compare the current state of the application in Kubernetes   with the desired state defined in the repository
            - If there is a discrepancy, ArgoCD will automatically deploy the necessary changes to bring the application to the desired state.
            Prod
            - All changes in the PROD environment will be performed manually. We will disable the automatic synchronization feature of ArgoCD and the Git repository.

