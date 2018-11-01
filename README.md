# Groovy Optimized Sonarqube Docker Image

Sonarqube docker optimized for evaluating Groovy projects

```
docker run -d -p 9000:9000 --name sonarqube musketyr/sonarqube:6

```

Contains ready-to-deploy Elastic Beanstalk archive ([see releases](https://github.com/musketyr/docker-sonarqube/releases))

Uses [forked version of Sonar Groovy Plugin](https://github.com/musketyr/sonar-groovy).