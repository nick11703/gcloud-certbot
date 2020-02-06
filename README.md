# Google Cloud Build for Let's Encrypt Certbot with Google Cloud DNS

## Running A build
```
gcloud builds submit --config cloudbuild.yaml
```

## Encrypting files
```
gcloud kms encrypt \
  --location global \
  --keyring [KEYRING] \
  --key [KEY] \
  --plaintext-file [FILE_TO_ENCRYPT] \
  --ciphertext-file [NEW_FILE_NAME]
```

## Add access to keys for cloud build service account 
```
gcloud kms keys add-iam-policy-binding \
  [KEY] --location=global --keyring=[KEYRING] \
  --member=serviceAccount:[CLOUD_BUILD_SERVICE_ACCOUNT_EMAIL] \
  --role=roles/cloudkms.cryptoKeyDecrypter
```

## Encrypting environment variables
```
echo "text to encrypt" | gcloud kms encrypt \
  --plaintext-file=- \
  --ciphertext-file=- \
  --location=global \
  --keyring=[KEYRING] \
  --key=[KEY] | base64
```
