# Create client with credentials profile

```python
import boto3

client = boto3.session.Session(profile_name="profile-name").client("foo-client")
```
