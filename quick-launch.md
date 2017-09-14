# Instantly laucnch a build on OSO

See also https://docs.openshift.com/online/using_images/s2i_images/java.html#s2i-images-java-deploy-applications-from-source

```
oc new-app redhat-openjdk18-openshift~<REPO_URL> \
	--context-dir=. \
	--build-env='ARTIFACT_DIR=<MAVEN_MODULE/target>' \ 
	--build-env='MAVEN_ARGS=install -pl <MAVEN_MODULE|.>'
```

NOTE: This does not expose a route automatically

## Exposing a route manually

```
oc expose svc/<SERVICE_NAME>
```
