# Self-hosted Broker

{% hint style="danger" %}
This page is deprecated. You can check the active version from [here](https://docs.serverlessdebugger.com/).
{% endhint %}

A self-hosted instance of the broker that provides the necessary functionality for online debugging can be installed on your own AWS account using our [CloudFormation template](self-hosted-broker.md#cloudformation-template) or you can perform a [manual installation](self-hosted-broker.md#manual-installation) using the Docker image we provide.&#x20;

You can configure the self-hosted broker with different access settings as described in below image:&#x20;

![Possible Configurations of Self-Hosted Debug Broker](<../../.gitbook/assets/Self-hosted Lambda Debug Broker.png>)

## **CloudFormation Template**

The Amazon CloudFormation template provides all the required resources for setting up the online debugging broker. You can easily install it on your own AWS account following the steps below. You will have both a public and an internal endpoint for the broker as the output. You can choose to have DNS mappings done automatically as subdomains under a Route53 hosted zone that you specify.

### **Installation Steps**

1. **SSL Certificate Preparation**\
   ****If you want to enable a secure connection via HTTPS, you will need an SSL certificate signed for the domains you are planning to use as the debugger endpoints. If you don’t have an available SSL certificate on AWS Certificate Manager, you can upload your existing certificate or request a new one by following this [guide](https://docs.aws.amazon.com/acm/latest/userguide/gs.html).\

2.  **Parameter Settings**\
    ****

    Create a CloudFormation stack by providing the following parameters using the  [template](https://thundra-dist.s3-us-west-2.amazonaws.com/thundra-debug-broker/thundra-debug-broker-onprem-setup-template.yml):

    |                                        | **Required** |    **Default value**   | **Description**                                                                                                                                                                                                                    |
    | -------------------------------------- | :----------: | :--------------------: | ---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
    | **License**                            |              |                        |                                                                                                                                                                                                                                    |
    | Thundra License Key                    |      YES     |            -           | Please contact us for the license key at [support@thundra.io](mailto:support@thundra.io)                                                                                                                                           |
    | **Debug Broker Instance**              |              |                        |                                                                                                                                                                                                                                    |
    | Version                                |      NO      |         latest         | The debugger version you want to deploy. Click [here](https://hub.docker.com/r/thundraio/thundra-debug-broker-onprem/tags) to see available versions                                                                               |
    | Amazon EC2 Instance Type               |      NO      |        t2.small        | Type of Amazon EC2 instance in which the broker will be run.                                                                                                                                                                       |
    | SSH IP Address Range                   |      NO      |        0.0.0.0/0       | IP address range that could be used for SSH access to the broker EC2 instance                                                                                                                                                      |
    | SSH Key Name                           |      YES     |            -           | EC2 KeyPair to enable SSH access to the broker EC2 instance                                                                                                                                                                        |
    | **Network Configuration**              |              |                        | ****:warning:**Please remember that both the Lambda functions you want to debug and your debugging environment should have access to the broker instance installed with these configurations.**                                    |
    | VPC                                    |      YES     |            -           | The VPC you want to deploy the debug broker                                                                                                                                                                                        |
    | Subnets                                |      YES     |            -           | :warning:**Two subnets in different AZs** under the selected VPC should be selected. Otherwise, the installation cannot be completed. Also, subnets should be able to connect to the internet so they can use the public endpoint. |
    | SSL Certificate ARN                    |      NO      |            -           | ARN of the SSL certificate you have on AWS Certificate Manager                                                                                                                                                                     |
    | **DNS Management**                     |              |                        |                                                                                                                                                                                                                                    |
    | Hosted Zone Name                       |      NO      |            -           | AWS Route53 hosted zone name for the automatic DNS configuration. You will need to write the host name without a period at the end (e.g., “thundra.io”).                                                                           |
    | Subdomain for Public Broker Endpoint   |      NO      |  thundra-debug-public  | The subdomain where the public broker will be served.                                                                                                                                                                              |
    | Subdomain for Internal Broker Endpoint |      NO      | thundra-debug-internal | The subdomain where the private broker will be served                                                                                                                                                                              |


3. **DNS Configuration**

{% hint style="success" %}
This configuration is created automatically if you have provided a “Hosted Zone Name” parameter.
{% endhint %}

{% hint style="info" %}
This configuration is not mandatory if you don’t provide an SSL certificate. Without the certificate, you can directly use the endpoints in the “Outputs” section.
{% endhint %}

You will see two URLs named BrokerInternalURL and BrokerPublicURL under the “Output” section after the stack creation is completed. You will need to update your DNS records to direct your reserved domains for the Thundra debugging broker to these endpoints.

For example: `thundra-debugger-public.yourdomain.com -> BrokerPublicURL`\
&#x20;`thundra-debugger-internal.yourdomain.com -> BrokerInternalURL`              &#x20;

### **Debug Tool Configuration**

Your debugging environment most likely has access to the internet and the best option at this point is to configure it to connect to the public broker endpoint. A secure connection is provided if you have an SSL certificate for the installation. If this is not the case or you want to use the internal endpoint anyway, you can access it from the debugging environment as long as it is connected to your VPC via a VPN tool.

You can use values under the “Output” section of the stack for the Thundra debugging client configuration. Use “BrokerPubliclUrl” or “BrokerInternalUrl” in order to debug with the public endpoint or the internal endpoint. Use “BrokerPort” as the broker host parameter.

{% hint style="warning" %}
Please note that if you make a manual DNS configuration, you should use the respective DNS record values as the broker host parameter instead of the values under the “Output” section.
{% endhint %}

### **Lambda Configuration**

Using the public endpoint is also the best option for your Lambda functions if they have internet access, but this may not be possible all the time. If you need to configure your Lambda functions to connect to the internal endpoint, you also need to be sure that you have an appropriate security configuration in order to access the debugging broker from your Lambda functions.

## **Manual Installation**

The official Thundra debugging broker Docker images can be accessed in this Docker Hub [repository](https://hub.docker.com/r/thundraio/thundra-debug-broker-onprem).&#x20;

You will need to pass the Thundra license key to the container on run. This can be done two ways:

1. Pass it as an environment variable named THUNDRA\_LICENSE\_KEY:\
   \
   `$ docker run -p 4444:4444 -p 5555:5555 --env THUNDRA_LICENSE_KEY=$THUNDRA_LICENSE_KEY thundraio/thundra-debug-broker-onprem:$BROKER_VERSION`\
   ****
2. Mount the folder containing .thundra\_license\_key file to bind the user home directory in the container:\
   \
   `$ docker run -p 4444:4444 -p 5555:5555 -v $PATH_TO_LICENSE_KEY_FOLDER:/root thundraio/thundra-debug-broker-onprem:$BROKER_VERSION`

&#x20;**Note that:**

* You should replace **$THUNDRA\_LICENSE\_KEY** with the license key you get from Thundra console for debugging. You can also put it in a file and use it as described in the second option.\
  &#x20;
* **$BROKER\_VERSION** can be replaced with any available version in the Docker Hub [repository](https://hub.docker.com/r/thundraio/thundra-debug-broker-onprem). You can also use "latest" to get the latest stable version.
