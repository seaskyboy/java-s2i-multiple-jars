# java-s2i-multiple-jars

## Summary

This example demonstrates one method of adding and using multiple JAR files in a container based on the [Red Hat Java S2I image](https://access.redhat.com/containers/#/registry.access.redhat.com/redhat-openjdk-18/openjdk18-openshift).  The application in this example is a binary build of this [example undertow servlet](https://github.com/jboss-openshift/openshift-quickstarts/tree/master/undertow-servlet).  In this example, I've added the [Glowroot](https://glowroot.org/) java APM agent as an additional JAR file to use.

The example consists of a [customized template](https://github.com/travisrogers05/java-s2i-multiple-jars/blob/master/openjdk18-web-basic-s2i-glowroot.yml) (openjdk18-web-basic-s2i-glowroot) based on the example [template provided by Red Hat](https://github.com/jboss-openshift/application-templates/blob/ose-v1.4.11/openjdk/openjdk18-web-basic-s2i.json).
The additions in this template are:
 - [Service object](https://github.com/travisrogers05/java-s2i-multiple-jars/blob/master/openjdk18-web-basic-s2i-glowroot.yml#L41#L54) representing the Glowroot endpoint
 - [Route object](https://github.com/travisrogers05/java-s2i-multiple-jars/blob/master/openjdk18-web-basic-s2i-glowroot.yml#L68-#L80) enabling ingress traffic to the Glowroot endpoint
 - [Environment variables](https://github.com/travisrogers05/java-s2i-multiple-jars/blob/master/openjdk18-web-basic-s2i-glowroot.yml#L142-#L146) to add the Glowroot agent to the JV start up


## Prerequisites
 - An Openshift environment ([Openshift Container Platform (OCP)](https://www.openshift.com/), [Origin](https://www.openshift.org/), or [Minishift](https://www.openshift.org/minishift/))
 - Openshift command line client
   - [OCP](https://access.redhat.com/downloads/content/290)
   - [Origin/Minishift](https://www.openshift.org/download.html)

## Instructions

Use the following commands to get the example up and running quickly:
~~~
oc login https://your.openshift.environment
oc new-project multiple-jar-test
oc create -f openjdk18-web-basic-s2i-glowroot.yml
oc new-app --template=openjdk18-web-basic-s2i-glowroot -p SOURCE_REPOSITORY_URL=https://github.com/travisrogers05/java-s2i-multiple-jars -p SOURCE_REPOSITORY_REF=master -p CONTEXT_DIR=""
~~~

Now use the URL of the Glowroot route to browse to the agents landing page running in the application pod.

An example might be:
~~~
http://openjdk-app-glowroot-multiple-jar-test.apps.example.com
~~~
