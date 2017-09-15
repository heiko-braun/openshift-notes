# Install HTTP
```   
mkdir /tmp/demo && /tmp/demo
git clone https://github.com/snowdrop/spring-boot-http-booster.git && cd booster
oc delete all --all
oc delete template launchpad-builder
oc create -f .openshiftio/application.yaml
oc new-app --template=launchpad-builder \
        -p SOURCE_REPOSITORY_URL=https://github.com/snowdrop/spring-boot-http-booster.git \
        -p SOURCE_REPOSITORY_REF=master \
        -p MAVEN_MIRROR_URL=http://nexus-infra.ose.spring-boot.osepool.centralci.eng.rdu2.redhat.com/nexus/content/groups/maven-redhat-public/
oc logs -f bc/spring-boot-http
cd .. && rm -rf booster
```

# Install Health Check
```
git clone https://github.com/snowdrop/spring-boot-health-check-booster.git && cd booster
oc delete all --all
oc delete template launchpad-builder
oc create -f .openshiftio/application.yaml
oc new-app --template=launchpad-builder \
        -p SOURCE_REPOSITORY_URL=https://github.com/snowdrop/spring-boot-health-check-booster.git \
        -p SOURCE_REPOSITORY_REF=master \
        -p MAVEN_MIRROR_URL=http://nexus-infra.ose.spring-boot.osepool.centralci.eng.rdu2.redhat.com/nexus/content/groups/maven-redhat-public/
oc logs -f bc/spring-boot-health-check
cd .. && rm -rf booster
```

# Install Config Map
```
git clone https://github.com/snowdrop/spring-boot-configmap-booster.git && cd booster
oc delete all --all
oc delete template launchpad-builder
oc create configmap app-config --from-file=src/main/etc/application.properties
oc policy add-role-to-user view -z default
oc create -f .openshiftio/application.yaml
oc new-app --template=launchpad-builder \
        -p SOURCE_REPOSITORY_URL=https://github.com/snowdrop/spring-boot-configmap-booster.git \
        -p SOURCE_REPOSITORY_REF=master \
        -p MAVEN_MIRROR_URL=http://nexus-infra.ose.spring-boot.osepool.centralci.eng.rdu2.redhat.com/nexus/content/groups/maven-redhat-public/
oc logs -f bc/spring-boot-configmap
cd .. && rm -rf booster
```

# Install CRUD
```
git clone https://github.com/snowdrop/spring-boot-crud-booster.git && cd booster
oc delete all --all
oc new-app -e POSTGRESQL_USER=luke -ePOSTGRESQL_PASSWORD=secret -ePOSTGRESQL_DATABASE=my_data openshift/postgresql-92-centos7 --name=my-database

oc delete template launchpad-builder
oc create -f .openshiftio/application.yaml
oc new-app --template=launchpad-builder \
        -p SOURCE_REPOSITORY_URL=https://github.com/snowdrop/spring-boot-crud-booster.git \
        -p SOURCE_REPOSITORY_REF=master \
        -p MAVEN_MIRROR_URL=http://nexus-infra.ose.spring-boot.osepool.centralci.eng.rdu2.redhat.com/nexus/content/groups/maven-redhat-public/
oc logs -f bc/spring-boot-crud
cd .. && rm -rf booster
```

# Install Circuit Breaker
```
git clone https://github.com/snowdrop/spring-boot-circuit-breaker-booster.git && cd booster
rm -rf ~/.m2/repository/io/openshift/booster/
mvn clean package -Popenshift -DskipTests -pl greeting-service
mvn clean package -Popenshift -DskipTests -pl name-service

oc delete all --all
oc delete template launchpad-builder-greeting-service
oc delete template launchpad-builder-name-service

oc create -f greeting-service/.openshiftio/application.yaml
oc create -f name-service/.openshiftio/application.yaml

oc new-app --template=launchpad-builder-greeting-service \
        -p SOURCE_REPOSITORY_URL=https://github.com/snowdrop/spring-boot-circuit-breaker-booster.git \
        -p SOURCE_REPOSITORY_REF=master \
        -p SOURCE_REPOSITORY_DIR=greeting-service \
        -p MAVEN_MIRROR_URL=http://nexus-infra.ose.spring-boot.osepool.centralci.eng.rdu2.redhat.com/nexus/content/groups/maven-redhat-public/
oc logs -f bc/springboot-cb-greeting

oc new-app --template=launchpad-builder-name-service \
        -p SOURCE_REPOSITORY_URL=https://github.com/snowdrop/spring-boot-circuit-breaker-booster.git \
        -p SOURCE_REPOSITORY_REF=master \
        -p SOURCE_REPOSITORY_DIR=name-service \
        -p MAVEN_MIRROR_URL=http://nexus-infra.ose.spring-boot.osepool.centralci.eng.rdu2.redhat.com/nexus/content/groups/maven-redhat-public/
oc logs -f bc/springboot-cb-name
cd .. && rm -rf booster
``` 
