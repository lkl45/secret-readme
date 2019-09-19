# Sealed Secrets

A Secret is an object that contains a small amount of sensitive data such as a password. Putting this information in a Secret object allows for more control over how it is used and reduces the risk of accidental exposure.  Secrets are simply encoded into Base 64 so the original values can easily be obtained.  

Sealed Secrets are composed of a cluster-side controller/operator and a client-side utility, **kubeseal**.  The **kubeseal** utility uses asymmetric crypto to encrypt secrets that only the controller can decrypt.  

The SealedSecret can only be decrypted by the controller running in the target cluster.  Therefore, Sealed Secrets can be safely stored in your repository and no one can obtain the original Secret from the Sealed Secret.

Sealed Secrets are located in this repository with the following structure:
```
<VSAD NAME>/  
├── <MICROSERVICE NAME>/  
│	 ├── <MICROSERVICE NAME>-sealedsecrets-<ENVIRONMENT NAME>.yaml
│	 ├── <MICROSERVICE NAME>-sealedsecrets-sit.yaml  
│	 └── <MICROSERVICE NAME>-sealedsecrets-stg.yaml
├──  microservice2  
│	 ├── microservice2-sealedsecrets-dev.yaml
│	 ├── microservice2-sealedsecrets-sit.yaml  
│	 └── microservice2-sealedsecrets-stg.yaml 
├── microservice3 
│	 ├── microservice3-sealedsecrets-dev.yaml
│	 ├── microservice3-sealedsecrets-sit.yaml  
│	 └── microservice3-sealedsecrets-stg.yaml
```

## Generating Sealed Secrets

Start with a Secret yaml file.  
```yaml
apiVersion: v1
kind: Secret
metadata:
	name: mysecret
	namespace: mynamespace
data:
	PASSWORD_A: dXNlcg==
```
Create a SealedSecret using the Verizon custom plugin:

	# Custom command for generating a SealedSecret
	$ kubeseal <mysecret.yaml >mysealedsecret.yaml #Update this custom command from verizon

The resulting SealedSecret that is stored in this repository will look like:

```yaml

```

**Note: The Secret and SealedSecret must have the same namespace and name.**

## Using Sealed Secrets 
Run command to apply secrets.

Secrets can be configured as environment variables or as files.

### Environment Variable
Create environment variables from SealedSecret

	# deployment yaml goes here  
	
Use the previously configured environment variables in Spring application properties file.
```properties
some.password=${SECRETS_PASSWORD_A}
```


**For more information see:**
[Kubernetes Secrets](https://kubernetes.io/docs/concepts/configuration/secret/),
[Sealed Secrets](https://github.com/bitnami-labs/sealed-secrets)