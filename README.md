# ai4os-secrets example

- Get access token from EGI Checkin or AI4EOSC IAM and setting it to ACCESS_TOKEN environment
```commandline
export ACCESS_TOKEN=<ACCESS_TOKEN>
```
- Install hvac and PyJWT
```commandline
pip install hvac pyjwt
```
- Comment/uncomment correct server setting according to Identity provider in the file "secrets.py"
- Execute the example
```commandline
python secrets.py
```


