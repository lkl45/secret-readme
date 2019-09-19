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
		name: mysecret
		namespace: mynamespace
	data:
		PASSWORD_A: dXNlcg==

Execute the following:

	# This is the important bit:
	$ kubeseal <mysecret.yaml >mysealedsecret.yaml

	# mysealedsecret.yaml is safe to upload to github, etc
	$ kubectl create -f mysealedsecret.yaml

	# Get the secret
	$ kubectl get secret mysecret

SealedSecret stored in source control will look like:

	apiVersion: bitnami.com/v1alpha1
	kind: SealedSecret
	metadata:
		name: mysecret
		namespace: mynamespace
	spec:
		encryptedData:
			PASSWORD_A: AgBy3i4OJSWK+PiTySYZZA9rO43cGDEq.....

Note the SealedSecret and Secret must have the same namespace and name.

## Using Sealed Secrets 
Secrets can be configured as environment variables or as files.

Create environment variables from SealedSecret
	deployment yaml goes here  
	
Use the previously configured environment variables in Spring application properties file.

	some.password=${SECRETS_PASSWORD_A}



**For more information see:**
[Kubernetes Secrets](https://kubernetes.io/docs/concepts/configuration/secret/),
[Sealed Secrets](https://github.com/bitnami-labs/sealed-secrets)