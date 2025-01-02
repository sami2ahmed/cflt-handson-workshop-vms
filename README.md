# cflt-handson-workshop
# Hands-On Workshop VMs

Welcome to the Hands-On Workshop VMs setup guide. This guide is for SEs at Confluent that wish to easily spin up VMs at scale for customers when they conduct workshops. Along with getting lots of VMs easily created, the Confluent Python client will be preconfigured on each VM automatically, so people new to the client can get started very quickly. I made this workshop within the limitations of a highly-regulated industry that could not have their developers SSH into VMs. Instead they used https://sshwifty-demo.nirui.org/ to SSH into the VMs. Example of customer-facing accompaniment to this guide can be found here: https://github.com/sami2ahmed/confluent-python-workshop. Also there is an example of the python client in the `python-client` dir of this repo. In the end, this repo allows SEs to create lots of VMs quickly, get a Confluent client dependencies installed on those VMs, so that a 1:m can be easily conducted for those new to Confluent. The Confluent python client was selected because this was for a data scientist/ML crowd most familiar with python. 

After completing this setup, you will have configured virtual machines in AWS for use in any workshop really, simply by updating the values in the `workshop-aws.yaml` file of this repo (i.e. `/cflt-handson-workshop-vms/myworkshop/workshop-aws.yaml`)

## Prerequisites

- Ensure you have access to the AWS Management Console. Open a "Assist" ticket in Slack in case you need access. 
- Install Python 3 on your local machine.
- Have your AWS credentials configured.

## Instructions

1. **Clone this Repository**

    ```sh
    git clone https://github.com/your-repo/workshop-vms.git
    cd workshop-vms/cflt-handson-workshop-vms
    ```

2. **Open the `workshop-aws.yaml` File**

    Open the `workshop-aws.yaml` file located in the `cflt-handson-workshop-vms/myworkshop` directory.

3. **Update the YAML Fields**

    Update the following fields in the `workshop-aws.yaml` file with your specific values. Use the provided examples as a reference.
    **please note** if you are also working in highly regulated industry that does not allow developers to SSH into VMs, then I highly suggest not editing the bootstrap VM script i.e. `cflt-handson-workshop-vms/core/terraform/common/bootstrap_vm.tpl` and sticking with this AWS ami: `ami-0950bf7d28f290092`. I encountered lots of trial and error with the sshwifty tool to get the confluent-python client working appropriately with different amis etc.  The workshop cloud provider & VM configuration, workshop leader you need to grab your unique access_key and secret_key from your AWS profile. The below are just example values (not real key/secret). 


 ```yaml
  name: 
  participant_count: 
  participant_password: 
  workshop-core
  core:
    cloud_provider: aws
    access_key: xxxx
    secret_key: xxxx
    profile: default
    region: ap-southeast-1
    # Availability zones
    availability_zones:
      - ap-southeast-1a
      - ap-southeast-1b
      - ap-southeast-1c
    vm_type: t2.micro
    vm_disk_size: 100
    ami: ami-0950bf7d28f290092
 ```

4. **Deploy the VMs**

    Use the following command to deploy the virtual machines:

    ```sh
    python3 workshop-create.py --dir myworkshop
    ```

Note that this will kick off a Terraform script and you will need to confirm with a `yes` to proceed (review all the AWS resources that will be spun up). If everything goes to plan you should see some output like: 
    ```
    Enter a value: yes
    module.workshop-core.random_string.random_string: Creating...
    module.workshop-core.random_string.random_string: Creation complete after 0s [id=xxxxxx]
    ```

The part that will take awhile, is the the remote-exec which will install librdkafka dependencies and everything required for the participant to kick off the python client easily. The file can be found here: `cflt-handson-workshop-vms/core/terraform/common/bootstrap_vm.tpl` You will notice it is doing some strange things like installing an older vversion of the confluent-kafka client. I had to do this in order for compatibility with the sshwifty tool I referred to in the top of this repo. Sorry not sorry.  

5. **Verify the Deployment**

After the deployment is complete, verify the deployment by checking on the VMs in your AWS Management Console. Or simply try SSH-ing into one of the VMs i.e. `ssh dc01@13.215.154.100`. There's an example of successfully logging into the VMs on the customer facing accompaniment i.e. https://github.com/sami2ahmed/confluent-python-workshop?tab=readme-ov-file#ssh. After you ssh into the VM, when you `ls` you should see: `get-pip.py` and `librdkafka` preinstalled on your VM. You can then follow the customer facing steps here for creating the python client on your VM and producing some data to your Confluent cluster: https://github.com/sami2ahmed/confluent-python-workshop?tab=readme-ov-file#confluent-python-client.  

## Teardown and Conclusion

Assuming you were able to successfully ssh into one of the VMs, you have successfully setup your virtual machines for a workshop. If you encounter any issues, please reach out to Sami Ahmed (SG timezone) in Slack.

    Use the following command to destroy the workshop:

    ```sh
    python3 workshop-destroy.py --dir myworkshop
    ```

## Troubleshooting

- Ensure all the fields in the `workshop-aws.yaml` file are correctly updated.
- Check your AWS credentials.
- Refer to the AWS documentation for any specific errors encountered during deployment.



