Purchase domain name (from google domain service)
Configure new domain name using google domain (map domain name with K8s ip address)
install cert manager into k8s prod cluster to set up the infra for cluster to communicate with letsencrypt to get certificate for the domain:

- use Helm to install cert manager
	- run install cmd in gcloud shell
	- will create cert manager pod/deployment to set up

- write yaml files for the cert manager
	- issuer yaml: where to go to attempt to get certificate
	- certificate yaml: info about the certificate we want to get, including the k8s sceret object to hold the certificate

- apply issuer and certificate yaml files so that cert manager can automatically reach out letsencrypt to get certificate

- verify k8s cluster has obtained certificate (should see the sceret in cloud console)

- reconfigure nginx ingress to serve https traffic

