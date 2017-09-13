# Nexus Setup

## Deploy Nexus

Mostly like this:

https://docs.openshift.com/container-platform/3.5/dev_guide/app_tutorials/maven_tutorial.html

```
oc new-app sonatype/nexus
oc expose svc/nexus
oc get routes
```

### Adding persistence

NOTE: It might require changes to minishift, see Appendix A

```
oc volumes dc/nexus --add \
	--name 'nexus-volume-1' \
	--type 'pvc' \
	--mount-path '/sonatype-work/' \
	--claim-name 'nexus-pv' \
	--claim-size '1G' \
	--overwrite
```

### Configure Nexus Proxies

http://nexus-myproject.192.168.64.3.nip.io/nexus

- User: admin
- Pass: admin123

Add the following proxies:

- https://repository.jboss.org/nexus/content/groups/public
- https://maven.repository.redhat.com/earlyaccess/all/

NOTE: Make sure to expose these proxies through the global public repository URL in Nexus

# Change the application template to use MAVEN_MIRROR_URL

Parameters of `Template`:

```
- name: MIRROR_URL
  description: The maven repo url, can be a nexus URL
  displayName: Mirror URL
  value: http://nexus.<PROJECT_NAME>:8081/nexus/content/groups/public
  required: true
[...]
```

Env Vars in `BuildConfig`:

```
env:
     - name: MAVEN_MIRROR_URL
       value: ${MIRROR_URL}
[...]
```

# Appendix A: Minishift Persistence

See also https://developers.redhat.com/blog/2017/04/05/adding-persistent-storage-to-minishift-cdk-3-in-minutes/

```
$ minishift ssh
[docker@minishift ~]$ sudo -i
[root@minishift ~]#
```

```
[root@minishift ~]# mkdir -p /mnt/sda1/var/lib/minishift/openshift.local.volumes/pv
[root@minishift ~]# mkdir /mnt/sda1/var/lib/minishift/openshift.local.volumes/pv/registry
[root@minishift ~]# chmod 777 -R /mnt/sda1/var/lib/minishift/openshift.local.volumes/pv
```

This should allocate 100 volumes underneath
