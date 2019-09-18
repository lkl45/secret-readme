# Secrets  

# Sealed Secrets

Sealed Secrets are composed of a cluster-side controller/operator and a client-side utility, kubeseal.  The kubeseal utility uses asymmetric crypto to encrypt secrets that only the controller can decrypt.  

The SealedSecret can only be decrypted by the controller running in the target cluster.  Therefore, Sealed Secrets can be safely stored in your repository and no one can obtain the original Secret from the Sealed Secret.


Sealed Secrets are located in a repository with the following structure:
```
ER6V/  
├── dev/  
│	 ├── microservice1-secrets.yml
│	 ├── microservice2-secrets.yml  
│	 └── microservice3-secrets.yml
├──  sit  
│	 ├── microservice1-secrets.yml
│	 ├── microservice2-secrets.yml  
│	 └── microservice3-secrets.yml 
├── prod 
│	 ├── microservice1-secrets.yml
│	 ├── microservice2-secrets.yml  
│	 └── microservice3-secrets.yml
```


## Generating Sealed Secrets

TBD



For more information see: 
* [Kubernetes Secrets](https://kubernetes.io/docs/concepts/configuration/secret/)
* [Sealed Secrets](https://github.com/bitnami-labs/sealed-secrets)