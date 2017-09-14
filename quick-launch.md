# Instantly laucnh a build on OSO

```
oc new-app redhat-openjdk18-openshift~<REPO_URL> \
	--context-dir=. \
	--build-env='ARTIFACT_DIR=<MAVEN_MODULE/target>' \ 
	--build-env='MAVEN_ARGS=install -pl <MAVEN_MODULE|.>'
```
