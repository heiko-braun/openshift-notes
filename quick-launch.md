# Instantly laucnh a build on OSO

```
oc new-app redhat-openjdk18-openshift~<REPO_URL> \
	--context-dir=. \
	--build-env='ARTIFACT_DIR=artefacts' \ 
	--build-env='MAVEN_ARGS=install -pl <MAVEN_MODULE|.>'
```
