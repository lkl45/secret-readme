# Sealed Secrets

A Secret is an object that contains a small amount of sensitive data such as a password. Putting this information in a Secret object allows for more control over how it is used and reduces the risk of accidental exposure.  Secrets are simply encoded into Base 64 so the original values can easily be obtained.  

Sealed Secrets are composed of a cluster-side controller/operator and a client-side utility, **kubeseal**.  The **kubeseal** utility uses asymmetric crypto to encrypt secrets that only the controller can decrypt.  

The SealedSecret can only be decrypted by the controller running in the target cluster.  Therefore, Sealed Secrets can be safely stored in your repository and no one can obtain the original Secret from the Sealed Secret.

Sealed Secrets are located in this repository with the following structure:
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

		apiVersion: v1
		kind: Secret
		metadata:
			name: db-secret
		data:
			PASSWORD_A: dXNlcg==
			ANOTHER_PASSWORD: cDQ1NXcwcmQ=
	

Execute the following:

	# This is the important bit:
	$ kubeseal <mysecret.yaml >mysealedsecret.yaml

	# mysealedsecret.yaml is safe to upload to github, etc
	$ kubectl create -f mysealedsecret.yaml

	# Profit!
	$ kubectl get secret mysecret


## Using Sealed Secrets 

Environment variables
Spring configuration

**For more information see:**
[Kubernetes Secrets](https://kubernetes.io/docs/concepts/configuration/secret/),
[Sealed Secrets](https://github.com/bitnami-labs/sealed-secrets)