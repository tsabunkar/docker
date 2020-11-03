# Install and use SonarQube for Deep Code Quality Analysis

# Used to show the Graph, Reports in GUI format ==> SonarQube Server

- \$ docker pull sonarqube:7.9.4-community
- \$ docker run -d --name sonarqube -e SONAR_ES_BOOTSTRAP_CHECKS_DISABLE=true -p 9000:9000 sonarqube:7.9.4-community
- Log in to http://localhost:9000
  - login=admin
  - password=admin
- create new project
- Project Key: nestjs-test
- Display name: nestjs-test
- Provide a token: nestjs-example > Generate
- Copy Token (f97710545655ea5253852992e77138fe7c4d5042) > Continue
- Run analysis on your project: others (js/ts), computer - linux

---

# Steps to set-up sonar scanner using -> nodejs-file/npm-package

- \$ npm i -D sonarqube-scanner
- In scripts add - "sonar": "node sonar-project.js"
- add a file - sonar-project.js (root of the project)
- Copy following code :

```
const sonarqubeScanner = require('sonarqube-scanner');

sonarqubeScanner(
  {
    serverUrl: 'http://localhost:9000',
    options: {
      'sonar.sources': 'src',
      'sonar.tests': 'src',
      'sonar.inclusions': 'src/**/*.ts', // Entry point of your code
      'sonar.test.inclusions':
        'src/**/*.spec.ts,src/**/*.spec.jsx,src/**/*.test.js,src/**/*.test.jsx',
    },
  },
  () => {
    console.log('Error Occurred while scanning');
  },
);

```

- \$ npm run sonar

---

# Steps to set-up sonar scanner-cli (used to deep scan the code ) ==> SonarQube Scanner (Docker Approach)

## NOTE: [This Approach is not loading the src folder in sonar-qubes server]

- \$ docker pull sonarsource/sonar-scanner-cli:4.5
- \$ docker run \
   --rm \
   -e SONAR_HOST_URL="http://${SONARQUBE_URL}" \
    -v "${YOUR_REPO}:/usr/src" \
   sonarsource/sonar-scanner-cli:4.5

```
Example:

- \$ docker run \
   --rm \
   --network=host \
   -e SONAR_HOST_URL="http://127.0.0.1:9000" \
   -v  $(pwd):/root/src \
   sonarsource/sonar-scanner-cli:4.5 \
   -Dsonar.projectKey=nestjs-test-example \
   -Dsonar.inclusions=src/**/*.ts \
   -Dsonar.test.inclusions=src/**/*.spec.ts,src/**/*.spec.jsx,src/**/*.test.js,src/**/*.test.jsx \
   -Dsonar.ts.tslint.configPath=tslint.json \

```

# Another docker image for sonar-scanner ==> [newtmitch/docker-sonar-scanner]

- \$ docker pull newtmitch/sonar-scanner:4-alpine

```

- \$ docker run -ti -v $(pwd):/usr/src --link sonarqube:7.9.4-community newtmitch/sonar-scanner:4-alpine \
        -D sonar.host.url=http://sonarqube:9000 \
        -D sonar.scm.provider=git  \
        -D sonar.projectBaseDir=./src \
        -D sonar.sources=. \
        -D sonar.projectName='Test-Nestjs-Analysis'

```

- Then Visit --> http://localhost:9000

---

# Ref

- https://medium.com/swlh/nodejs-code-evaluation-using-jest-sonarqube-and-docker-f6b41b2c319d
- https://www.ryandoll.com/post/2018/3/25/sonarqube-docker
- https://docs.sonarqube.org/latest/analysis/scan/sonarscanner/
- https://hub.docker.com/r/sonarsource/sonar-scanner-cli
- https://medium.com/swlh/sonarqube-sonarscanner-setup-1a633b654828
- https://medium.com/@HoussemDellai/setup-sonarqube-in-a-docker-container-3c3908b624df
- https://github.com/newtmitch/docker-sonar-scanner
- https://github.com/bellingard/sonar-scanner-npm

---
