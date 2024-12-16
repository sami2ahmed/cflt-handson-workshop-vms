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
    **please note** if you are also working in highly regulated industry that does not allow developers to SSH into VMs, then I highly suggest not editing the bootstrap VM script i.e. `cflt-handson-workshop-vms/core/terraform/common/bootstrap_vm.tpl` and sticking with this AWS ami: `ami-0950bf7d28f290092`. I encountered lots of trial and error with the sshwifty tool to get the confluent-python client working appropriately with different amis etc. 


    ```yaml
  name: 
 
  # The number of people attending the workshop ssh password
  participant_count: 7
  participant_password: mycoolpassword

  #
  # workshop-core
  #
  core:

    # The workshop cloud provider & VM configuration, workshop leader you need to grab your unique access_key and secret_key from your AWS profile. The below are just example values (not real key/secret). 
    cloud_provider: aws
    access_key: XXXXXXXXXXXXXXXXXXXXXX
    secret_key: XXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXX
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
    module.workshop-core.random_string.random_string: Creation complete after 0s [id=]
    module.workshop-core.data.template_file.aws_ws_iam_name: Reading...
    module.workshop-core.data.template_file.aws_ws_iam_name: Read complete after 0s [id=a061ae14b562ce14fd91c6e9fe6587ee2fb871f1ef231d54d8aa742b9669eea1]
    module.workshop-core.aws_iam_user.ws: Creating...
    module.workshop-core.aws_vpc.workshop-vpc: Creating...
    module.workshop-core.aws_iam_user.ws: Creation complete after 2s [id=tf-user-userconfluentworkshop-awyigwzy]
    module.workshop-core.aws_iam_access_key.ws: Creating...
    module.workshop-core.aws_iam_access_key.ws: Creation complete after 0s [id=]
    module.workshop-core.local_file.aws_credentials: Creating...
    module.workshop-core.local_file.aws_credentials: Creation complete after 0s [id=132db1e35babd2c53b21ac00a8f08f99a85a9f4a]
    module.workshop-core.aws_vpc.workshop-vpc: Still creating... [10s elapsed]
    module.workshop-core.aws_vpc.workshop-vpc: Creation complete after 12s [id=vpc-01bc53db723e87a49]
    module.workshop-core.aws_route_table.workshop-public-route-table: Creating...
    module.workshop-core.aws_internet_gateway.workshop-ig: Creating...
    module.workshop-core.aws_subnet.workshop-public-subnet[1]: Creating...
    module.workshop-core.aws_subnet.workshop-public-subnet[0]: Creating...
    module.workshop-core.aws_security_group.instance: Creating...
    module.workshop-core.aws_internet_gateway.workshop-ig: Creation complete after 0s [id=igw-0814ad3414f6b3e47]
    module.workshop-core.aws_route_table.workshop-public-route-table: Creation complete after 0s [id=rtb-02dde8dc538ecdc4c]
    module.workshop-core.aws_route.workshop-public-route: Creating...
    module.workshop-core.aws_route.workshop-public-route: Creation complete after 1s [id=r-rtb-02dde8dc538ecdc4c1080289494]
    module.workshop-core.aws_security_group.instance: Creation complete after 2s [id=sg-0dcb04edb96aeabcc]
    module.workshop-core.aws_subnet.workshop-public-subnet[0]: Still creating... [10s elapsed]
    module.workshop-core.aws_subnet.workshop-public-subnet[1]: Still creating... [10s elapsed]
    module.workshop-core.aws_subnet.workshop-public-subnet[1]: Creation complete after 11s [id=subnet-0ea6f9b59303537d6]
    module.workshop-core.aws_subnet.workshop-public-subnet[0]: Creation complete after 11s [id=subnet-0a3f5fc29eb7ccabd]
    module.workshop-core.aws_route_table_association.workshop-public-route-table-association[0]: Creating...
    module.workshop-core.aws_route_table_association.workshop-public-route-table-association[1]: Creating...
    module.workshop-core.aws_instance.instance[0]: Creating...
    module.workshop-core.aws_route_table_association.workshop-public-route-table-association[0]: Creation complete after 0s [id=rtbassoc-0fa3e384bbd5ae7e1]
    module.workshop-core.aws_route_table_association.workshop-public-route-table-association[1]: Creation complete after 0s [id=rtbassoc-0b98c5144973d323c]
    module.workshop-core.aws_instance.instance[0]: Still creating... [10s elapsed]
    module.workshop-core.aws_instance.instance[0]: Creation complete after 12s [id=i-0a709cb6c80f0ff6e]
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



