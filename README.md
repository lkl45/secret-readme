# Secrets  

A Secret is an object that contains a small amount of sensitive data such as a password. Putting this information in a Secret object allows for more control over how it is used and reduces the risk of accidental exposure.

To use a secret, a pod needs to reference the secret. A secret can be used with a pod in two ways: as files in a volume mounted on one or more of its containers, or used by kubelet when pulling images for the pod.

# Sealed Secrets

Sealed Secrets are composed of a cluster-side controller/operator and a client-side utility, kubeseal.  The kubeseal utility uses asymmetric crypto to encrypt secrets that only the controller can decrypt.  

The SealedSecret can only be decrypted by the controller running in the target cluster.  Therefore, Sealed Secrets can be safely stored in your repository and no one can obtain the original Secret from the Sealed Secret.

Sealed Secrets are located in a repository with the following structure:
```
<VSAD NAME>/  
├── <ENVIRONMENT NAME>/  
│	 ├── microservice1-sealedsecrets-<ENVIRONMENT NAME>.yaml
│	 ├── microservice2-sealedsecrets-<ENVIRONMENT NAME>.yaml  
│	 └── microservice3-sealedsecrets-<ENVIRONMENT NAME>.yaml
├──  sit  
│	 ├── microservice1-sealedsecrets-sit.yaml
│	 ├── microservice2-sealedsecrets-sit.yaml  
│	 └── microservice3-sealedsecrets-sit.yaml 
├── stg 
│	 ├── microservice1-sealedsecrets-stg.yaml
│	 ├── microservice2-sealedsecrets-stg.yaml  
│	 └── microservice3-sealedsecrets-stg.yaml
```

## Generating Sealed Secrets

Start with a Secret yaml file.

	# This is the important bit:
	$ kubeseal <mysecret.yaml >mysealedsecret.yaml

	# mysealedsecret.yaml is safe to upload to github, etc
	$ kubectl create -f mysealedsecret.yaml

	# Profit!
	$ kubectl get secret mysecret


## Using Sealed Secrets 
TBD

For more information see: 
* [Kubernetes Secrets](https://kubernetes.io/docs/concepts/configuration/secret/)
* [Sealed Secrets](https://github.com/bitnami-labs/sealed-secrets)