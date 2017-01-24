# Customer0 PCF+GCP Concourse Pipeline


### Pre_Reqs & Instructions for POC Deployment via Concourse

1. Create a Service Account with "Editor" Role on the target GCP Project
  - Non POC versions of the pipeline will support sep Terraform & Opsman Service accounts
  - ENABLE your GCP Compute API [here](https://console.cloud.google.com/apis/api/compute_component)
  - ENABLE your GCP Storage API [here](https://console.cloud.google.com/apis/api/storage_component)
  - ENABLE your GCP SQL API [here](https://console.cloud.google.com/apis/api/sql_component)
  - Enable GCP SQL admin API https://github.com/c0-ops/gcp-concourse
  - ENABLE your GCP DNS API [here](https://console.cloud.google.com/apis/api/dns)
  - ENABLE your GCP Cloud Resource Manager API [here](https://console.cloud.google.com/apis/api/cloudresourcemanager.googleapis.com/overview)
  - ENABLE & Create GCP Storage Interoperability Tokens here [here](https://console.cloud.google.com/storage/settings)
2. Create a Concourse instance with public access for downloads.  Look [here](http://concourse.ci/vagrant.html) for `vagrant` instructions if an ephemeral concourse instance is desired.


3. `git clone` this repo
4. **EDIT!!!** `ci/c0-gcp-concourse-poc-params.yml` and replace all variables/parameters you will want for your concourse individual pipeline run

   - The sample pipeline params file includes 2 params that set the major/minor versions of OpsMan & ERT that will be pulled.  They will typically default to the latest RC/GA avail tiles.
     ```
     opsman_major_minor_version: '1\.9\..*'
     ert_major_minor_version: '1\.9\..*'
     ```
     - change all the required fields
     - don't forget to change the project ID.
     - copy paste the content of the json file of your service account to the gcp_svc_acct_key field in the following format. Notice the space before "{" and the space before "}". 
     gcp_svc_acct_key: |
 {
  "type": "service_account",
  "project_id": "fe-jmclaughlin",
  "private_key_id": "d548262a95fbd16b94a7bc5c3c44def35e02d35b",
  "private_key": "-----BEGIN PRIVATE KEY-----\nMIIEvwIBADANBgkqhkiG9w0BAQEFAASCBKkwggSlAgEAAoIBAQDIS44WVcm128Wi\n+PVcCqX6RuXRoLNo1gUt0iOyCOKVrwsyD8zNxWMgPq2lk0PeM2H+bQwFTUxP1XJL\nmeEHtOOhnHnjYGo4aV9pgHoP98VQEUJfh0SriZcHjU+KGAHgW7Ib6zlp5S4oQ0kF\n13/6+o5BxUn8L0zXNhixKu8ulGzRXV7nrV+Of1NqXaNaLldfzgDIw6CnJn/nK/Cn\nkxc2+Q3oueZPogRb24GCiez3V9uAU2OfSX1Lx63/hQMD1DIgiklHAfEEv/BkhH00\n+pGzDvcSHohF09Yuoas+VQUgu5QDUbrCc9Jw76V0EbDOQp3Qf9IFreDndVYAEWGF\n0Gc8L5pzAgMBAAECggEBAMUTsoa/em16BQjKNYGO6KlNwSt2F5F7pDTlo0G2BFyL\nk1R6v2VoZpR/l5RnRkwH+s/AtCczW3bh6kgA7K4Mij2mHThg0aMX601/oJq9jGOv\n18Lu8d5mzzgbDrwtywrarnFSDXfojHYJXnxlAgQNLJQCbz23vL+09q68NAN8/2Uv\n36ViWA3zz8T0K7aDxh9eFwZJKLdP/xnHzhWi1N1//0Y5g1SfSnZSWs1AdywhYrz1\nSknqIaFr4KTe4uKSBmbTCoZC7VrRARieogMPboiQll8gdwnFV1lEzLxDP+wMgf/m\nxmrtgmlYt40paPA+RUMmPhcfT4RJzoXpHBlj8OV4v8ECgYEA8Bjy5aKeMJYntyxT\nvD4JW8vMAres/Q+HLHOcoBJoo7ewMUpOa1lRimX3an36yGEl+/N2EQAT99j4vQKD\n7WalEiDmyKey6PuH3AoqVAQtu5Yekp8MSPlia2lsirBdn2/D58hYsbD2xFOD8z+f\nFauzUQycBTYbEN9LiB9nNrROWisCgYEA1Y+5lM4OuG4210lP9LrRXuSLENtBtyTK\n5QViIxrjzBEFWY9YJJqR5pka/uwsT953dLpGYSZYshEEQ1TeuSd+JkNqJVbhT9kJ\nWtQSV7nnafM7FndkcM3qeXSk6JWy779M0YZv36mmA7TF3hq8B03uBXgu0Ancqig4\n0+E5161VhNkCgYEArLil9ECaIEXE6GcBDghq3xiq+MF9tsb27Sl2YUkc8bnxDGRy\nKZOlrzRPWtKqGICavLeWFgDCXKg/uGkY0y3mTjZRD8RkVmqsf8ToUmx3Id2KvNui\nENUm0jKTHOpnT40tl45vD9VIkE+sOs9n+ET+yK2Th8Q2kFqykYhVzerD+uUCgYEA\nkZU8qZgeqNNZR0GO5AJGoC2kL4WIMtU+Cwm0cMHv8DjaMMdrCujj9RMCOC2/t2Ks\nhEJHoAqIBDtdcJj2i7nEYUkrnvCu/8OwgN548pykiLFq4lHZgpyc7tb5ZCRIqu75\n6wt+UDZSGcyt5k7LRx901v2qy98tMkHhG286AzECT2ECgYBq/qzYULl+HcZ+p0ex\nT0V8QRt0gwX5DLhXwWeMri3e1n8Oyx+sMtdVgPy0pbcsk7pKnqnihWOlwC4d1gJG\nrfPPNGxZp39m3GWYUUAkPyqJq61JPdhTn7uN+Fgh0lQAYZSjlq4dIz6MpuRls9VK\nNseZXQejb0k/rgQSukoKGWU2gg==\n-----END PRIVATE KEY-----\n",
  "client_email": "jmconcourse@fe-jmclaughlin.iam.gserviceaccount.com",
  "client_id": "108747227887076707784",
  "auth_uri": "https://accounts.google.com/o/oauth2/auth",
  "token_uri": "https://accounts.google.com/o/oauth2/token",
  "auth_provider_x509_cert_url": "https://www.googleapis.com/oauth2/v1/certs",
  "client_x509_cert_url": "https://www.googleapis.com/robot/v1/metadata/x509/jmconcourse%40fe-jmclaughlin.iam.gserviceaccount.com"
 }

5. **AFTER!!!** Completing Step 4 above ... log into concourse & create the pipeline.

  _(this command syntax assumes you are at the root of your repo)_
  - `fly -t [YOUR CONCOURSE TARGET] set-pipeline -p c0-gcp-concourse-base -c ci/c0-gcp-concourse-poc.yml -l ci/c0-gcp-concourse-poc-params.yml`

6. Un-pause the pipeline
   >fly -t [YOUR CONCOURSE TARGET] unpause-pipeline -p c0-gcp-concourse-base
7. go to the console and Run the **`init-env`** job manually,  you will need to review the output and record it for the DNS records that must then be made resolvable **BEFORE!!!** continuing to the next step:
  - Example:

```
==============================================================================================
This gcp_pcf_terraform_template has an 'Init' set of terraform that has pre-created IPs...
==============================================================================================
Activated service account credentials for: [c0-concourse@pcf-demos.google.com.iam.gserviceaccount.com]
Updated property [core/project].
Updated property [compute/region].
You have now deployed Public IPs to GCP that must be resolvable to:
----------------------------------------------------------------------------------------------
*.sys.gcp-poc.customer0.net == 130.211.9.202
*.cfapps.gcp-poc.customer0.net == 130.211.9.202
ssh.sys.gcp-poc.customer0.net == 146.148.58.174
doppler.sys.gcp-poc.customer0.net == 146.148.58.174
loggregator.sys.gcp-poc.customer0.net == 146.148.58.174
tcp.gcp-poc.customer0.net == 104.198.241.71
opsman.gcp-poc.customer0.net == 104.154.98.48
----------------------------------------------------------------------------------------------
```

**[DEPLOY]**. **AFTER!!!** Completing Step 7 above ... Run the **`deploy-iaas`** job manually, if valid values were passed, a successful ERT deployment on GCP will be the result.
