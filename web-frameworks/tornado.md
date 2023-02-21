# Tornado

[Tornado](https://www.tornadoweb.org/en/stable/) is a Python web framework and asynchronous networking library, originally developed at [FriendFeed](https://en.wikipedia.org/wiki/FriendFeed). Tornado has been supported for python 3.7 and above by Thundra.

## Step 1: Install Thundra

Install `thundra` to your environment by running:

```
pip install --upgrade thundra
```

### Step 2: Configure Integration

Initialize and configure thundra in your before application file before tornado app is initialized. After that thundra will automatically trace all your tornado requests.

```
import thundra

# You can provide configuration options with thundra.configure
# Configure thundra.apikey in here or by setting
# thundra_apiKey environment variable while running your application
thundra.configure(
    options={
        "config": {
            "thundra.apikey": "<your-api-key-here>",
            "thundra.agent.application.name": "sample_app",  # Sample configuration, not required
        }
    }
)
```

#### **Additional Configurations:**

**Enabling Request Body Tracing**

Http request body is not sent to thundra by default. To be able to trace the request body to your endpoints, you can set `THUNDRA_TRACE_REQUEST_SKIP` environment variable to `false` or by providing it to options.config as in the below configuration.

```
import thundra

# You can provide configuration options with thundra.configure
# Configure thundra.apikey in here or by setting
# thundra_apiKey environment variable while running your application
thundra.configure(
    options={
        "config": {
            "thundra.apikey": "<your-api-key-here>",
            "thundra.agent.application.name": "sample_app",  # Sample configuration, not required
            "thundra.agent.trace.request.skip": False
        }
    }
)
```
