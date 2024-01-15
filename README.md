# ai4os-secrets example

## How to run the example

- Get an access token from EGI Checkin or AI4EOSC IAM and setting it to the ACCESS_TOKEN environment
```commandline
export ACCESS_TOKEN=<ACCESS_TOKEN>
```
- Install hvac and PyJWT library
```commandline
pip install hvac pyjwt
```
- Comment/uncomment the correct server setting according to the Identity provider 
(EGI Checkin Prod/Dev/Demo or AI4EOSC IAM) in the file "secrets.py". For example for AI4EOSC IAM
```python
VAULT_ADDR = "https://secrets.services.ai4os.eu:8200"
VAULT_AUTH_PATH = "jwt"
VAULT_ROLE = ""
VAULT_MOUNT_POINT = "/secrets/"
```

- Execute the example
```commandline
python secrets.py
```

# How the code works

Each operation is divided into three steps:

- Initialize the client

```python
client = hvac.Client(url=VAULT_ADDR)
client.auth.jwt.jwt_login(role=VAULT_ROLE, jwt=access_token, path=VAULT_AUTH_PATH)
```

- Call the corresponding function of the client for the required operation (create/list/read/delete secrets).
The most important parameter is the path what is composed of home path + local secret path
```python
# Create/update a secret
client.secrets.kv.v1.create_or_update_secret(
    path=home_path + "test01",
    mount_point=VAULT_MOUNT_POINT,
    secret={"username": "abcdef", "password": "123456"},
)

# Read a secret
response = client.secrets.kv.v1.read_secret(
    path=home_path + "test01",
    mount_point=VAULT_MOUNT_POINT,
)
```

- Extract output data from responses for listing and reading operations. Creating/deleting operations do not return 
any data 

```python
# Extract the list of secrets in the path from  response
secrets_list = map(str, response["data"]["keys"])

# Extract the secret data in dict {key:value} format from response
secret = response["data"]
```


