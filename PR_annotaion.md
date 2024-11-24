1. Task: https://github.com/rolling-scopes-school/tasks/blob/master/devops/modules/3_ci-configuration/task_6.md
2. Code: https://github.com/VasylKhytrych/Helm-Jenkins-WP
3. Tasks and my job review.

- **Pipeline Configuration (40 points)**
   - [x] A Jenkins pipeline is configured and stored as a Jenkinsfile in the main git repository.
   - [x] The pipeline includes the following steps: 
      - ***My comment:*** Please check repo for Jenkinsfile and all steps -> [here](https://github.com/VasylKhytrych/Helm-Jenkins-WP/blob/main/Jenkinsfile).

- **Artifact Storage (20 points)**
   - [x] Built artifacts (Dockerfile, Helm chart) are stored in git and ECR (Docker image).
      - ***My comment:*** Please check Dockerfile and Jenkinsfile where you can see that images in ECR and config in this repo files.

- **Repository Submission (5 points)**
   - [x] A repository is created with the application, Helm chart, and Jenkinsfile.
      - ***My comment:*** This is exactly this repo.
      
- **Verification (5 points)**
   - [x] The pipeline runs successfully and deploys the application to the K8s cluster.
     - ***My comment:*** Please check [SS](https://github.com/VasylKhytrych/Helm-Jenkins-WP/blob/main/deploy.png) from pipeline.

- **Additional Tasks (30 points)**
   - [x] Application Verification (10 points)
   - [x] Notification System (10 points)
   - [x] Documentation (10 points)
     - ***My comment:*** You can read README.md of this repo. You will receive emails about pipeline finish result. Verification possible via helm,kube,browser. 

  P.S. In case something is unclear or you need clarification to rate it, please contact me in the Discord -> @lunnoe_solnce.