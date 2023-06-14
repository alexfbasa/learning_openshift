# learning_openshift



<ol>Set up a service account and generate an authentication token in your OpenShift cluster:</ol>
    Log in to your OpenShift cluster using the oc command-line tool.
    Create a new service account: oc create serviceaccount jenkins-deployer
    Grant the necessary permissions to the service account: oc policy add-role-to-user edit system:serviceaccount:your-namespace:jenkins-deployer
    Retrieve the authentication token: oc serviceaccounts get-token jenkins-deployer

### Commands
```shell
oc create serviceaccount jenkins-deployer
```
````text output
serviceaccount/jenkins-deployer created
````
```shell
oc policy add-role-to-user edit system:serviceaccount:your-namespace:jenkins-deployer
```
````text output
clusterrole.rbac.authorization.k8s.io/edit added: "system:serviceaccount:your-namespace:jenkins-deployer"
````
```shell
oc serviceaccounts get-token jenkins-deployer
```
````text output
TOKEN
````
In your Jenkins instance, navigate to the Jenkins Dashboard.

Click on "Credentials" in the left-hand side menu.

Click on "System" in the main section.

Click on "Global credentials (unrestricted)".

Click on "Add Credentials".

Select the appropriate credential type (e.g., "OpenShift Token Credentials").

In the form, enter a meaningful ID for the credentials and the authentication token you obtained from OpenShift.

Save the credentials.

Now, in your Jenkins job configuration, you can use the OpenShift token credentials to authenticate with your local OpenShift cluster:

Add a build step or script to deploy the container to OpenShift.
Use the oc command-line tool with the token credentials to interact with the OpenShift cluster.
For example, you can use oc login your-openshift-endpoint --token $TOKEN to authenticate and oc apply -f your-deployment-config.yaml to apply the deployment configuration.
Remember to adjust the placeholders (your-namespace, your-openshift-endpoint, your-deployment-config.yaml) with your actual values.

By configuring the OpenShift token credentials in Jenkins, you can securely authenticate and interact with your local OpenShift cluster during the deployment process.