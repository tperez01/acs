**RHACS Simple installation:**

1. Install the ACS operator:
   
    1.1 Create the rhacs-operator namespace '''oc apply -f NamespaceOperator.yaml'''
    1.2 Install the operator '''oc apply -f Subscription.yaml'''

2. Create the ACS Central resource
   
    2.1 Create the stackrox namespace '''oc apply -f NamespaceCentral.yaml'''
    2.2 Create the resource '''oc apply -f Central.yaml'''

3. Test ACS Central (command line)
   
    3.1 Get the ACS portal link '''oc -n stackrox get route central -o jsonpath="{.status.ingress[0].host}" '''
    3.2 Get the password to the admin user '''oc -n stackrox get secret central-htpasswd -o go-template='{{index .data "password" | base64decode}}' '''
   Test ACS Central (Web console)
    3.1 Navigate to Networking → Routes
    3.2 Find the central Route and click on the RHACS portal link under the Location column

4. Create the init bundle, we need it as a means to communicate our clusters to ACS (mine it's not on the repo because it contains private keys)
   
    4.1 On the RHACS portal, navigate to Platform Configuration → Integrations
    4.2 Navigate to the Authentication Tokens section and click on Cluster Init Bundle
    4.3 Click Generate bundle
    4.4 Enter a name for the cluster init bundle and click Generate
    4.5 Donwload init bundle file

5. Apply the init bundle to the namespace where the central services are installed
   
    5.1 '''oc create -n <stackrox> -f <init_bundle>.yaml'''

6. Create the Secured Cluster resource
   
    6.1 Update the centralEndpoint specification with your ACS console URL in the SecuredCluster.yaml file
    6.2 Create the resource '''oc apply -f SecuredCluster.yaml'''

7. Verify instalation
   
    7.1 Log into the ACS web console
    7.2 Navigate to clusters
    7.3 Your secured cluster should be listed here

8. Enjoy! :D
