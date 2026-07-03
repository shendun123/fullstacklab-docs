# SDK Examples

## OpenAI Python SDK

```python
from openai import OpenAI

client = OpenAI(
    api_key="YOUR_API_KEY",
    base_url="https://fullstacklab.xyz/v1"
)

response = client.chat.completions.create(
    model="gpt-5.5",
    messages=[{"role": "user", "content": "Hello"}]
)

print(response.choices[0].message.content)
```

## REST Requests

For image and video task APIs, REST requests are recommended because they make it easier to print status codes, task IDs, and full error messages.

## Environment Variable Example

```python
import os

API_KEY = os.getenv("FULLSTACKLAB_API_KEY")
```

Windows PowerShell:

```powershell
[Environment]::SetEnvironmentVariable("FULLSTACKLAB_API_KEY", "your-real-key", "User")
```
