

# ................... 101 Terraform Interview Questions  ....................

## By  Mamun Rashid :: https://www.linkedin.com/in/mamunrashid/ :: Please connect with me ::

### Last Updated: 2021.03.09
###
###

*  ( I also have 275 Multiple Choice Questions for Hashicorp Terraform Associate Certification Exam found at the link below:
*    https://testmoz.com/1245195 )

*  ( Mr. Bachina has also made an execllent set, which is very effective, as well. That can be found at the link below:
*    https://medium.com/bb-tutorials-and-thoughts/250-practice-questions-for-terraform-associate-certification-7a3ccebe6a1a )

##
##

#### 1. Imagine that there are 20 people in the company who use Terraform that manages resources in AWS or GCP. How can you set up a system that tackles that? What could be the issues that can arise from this?


   Answer: 
   At least two issues arise when multiple people work on Terraform together touching the same resources:
         1. state file overrides
         2. code overrides
         State file overrides can be solved using remote states
         Code overroides can be solved usig git repos and branching strategies.


 ## 2. There are 5 people in your team that use Terraform. Some time in the last few days, a resource in AWS changed. How can you find out who did this?

  Answer: If it was done via Console, Cloudtrail can tell you. If it was done via Terraform code, 
git repo AND AWS Cloudtrail can tell you.


##  3. You made a module. One Terraform code uses that module. But, now, you improved that module, but the "caller" code is not compatible with the new version of the module.  How can you have both versions of the module in use?

  Answer: You can have verions on your modules and the caller code can refer to specific version of the module.In the "caller" code, specify the appropriate version using the source parameter, pointing to the correct module path or branch.


## 4. You have existing infrastructure in AWS that was NOT made by Terraform. How can you bring that infrastructure in Terraform code's control?

   Answer: If you all want is the "state" to be in Terraform, you can use "terraform import" commnad.If you also want terraform code be made, you can either do yourself (until code and state match exactly), or you can use a opensource tool named "Terraformer"

## 5. If N people are using Terraform, How can we make sure people don't bring up resources in AWS/GCP that are too expensive?

  Answer: There is proprietory tool call called Scalr. Or, you can use Terraform Enterprise. There may also be a way using Open Policy Agent. Cloud providers often provide tools for this, as well.
  You can enforce cost controls by setting budget alerts in AWS/GCP, using Terraform policies with Sentinel or Open Policy Agent (OPA), and defining resource constraints in Terraform configuration files.

##
 6. How can you tackle secrets in Terraform?

  Answer: Starting version 0.14, you can mark your variables "sensitive". This allows logs of these variables to be masked.  You can also integrate Vault with Terraform. Hashicorp Vault and Azure Key Vault



##
7. What is the big deal about Terraform v0.13 and above?

  Answer: Before version 0.13, programming logic was nearly impossible to implement in HCL. With 0.13, it is still not super easy, but a huge progress was made.  There were many other major features released as well (e.g. json output)



##
 8. You have 2 folders of terraform code. Folder A and Folder B. Folder B needs to use output (state) from folder A to create resources. How can you accomplish this?

  Answer: This has been long-standing problem with Terraform. Terragrunt is one way to get this done. Others have to implement custom Python scripts that copies states back and forthe between folders.To use outputs from Folder A in Folder B, configure Terraform remote state. Export Folder A’s state using terraform output and access it in Folder B using the terraform_remote_state data source.



##
9. Why would you need "data" resources in Terraform?

 Answer: To refer to resources that already exists  in AWS. For example, list of AMIs in a region.




#
 10. Is it safe to store terraform state in a private git repo? Why or why not?

 Answer: It is NOT safe to store in git repos, because it can hold secrets. Also, it is likely that people will override each other's changes in state.



##
 11. If you are tagged to implement Terraform in a team or company where they have never used Terraform, what issues might you solve pre-emptively?

  Answers: Set standars and best practices before you start coding. Also, you many want to import existing resources in Terraform before you start.

##
#### 12. You are going to deploy similar resources in Development, Staging and Prod environments. How can you code so that you can deploy to similar Terraform code with repeating your code.

  Answer: Have no hard-coded variables and use .tfvars file per environment.use workspaces or create reusable modules. Parameterize the configurations (e.g., using variables) define environment-specific values in separate tfvars files.



##
#### 13. How would you implement a Terraform pipeline?

   Answer: In the same folder where I have terraform files, I would have .gitlabci.yml file which will have various stages (e.g. tfsec, tflint, checkov, terraform validate,terrform plan and terarform apply and possibly some testing stages).
##
#### 14. Tell me about a project or task that you did in Terraform?

    Answer: Tell your story.


##
#### 15.  How do you import state from GCP or AWS?

    Answer: Using terraform import command. Format for each resource type may vary. Once you have successfully imported the state, you can also write terraform code to match the state so much your code and state and infrasture are all in sync.


##
### 16. Is there a tool for looking into many resources (> 100) in GCP or AWS and automatically made Terraform code for them?

    Answer: Terraformer, (an open source tool). It is magical.


##
#### 17.  How do you define "backend" in Terraform?

    Answer:  This is directly from Hashicorp Web Site:

             Backends are configured with a nested backend block within the top-level terraform block:


              terraform {
                backend "remote" {
                  organization = "example_corp"
              
                  workspaces {
                    name = "my-app-prod"
                  }
                }
              }

##
### 18. What is a provider and why do you need it?

    Answer: Terraform doesn't directly create resources in the cloud. It interacts with provider (given by AWS, GCP, etc.), which enables communication between Terraform and the cloud provider APIs (AWS, GCP etc.)Using the same idea, Terraform can also deploy resources in non-cloud applications as long as it has a provider (e.g. Hashicorp Vault). A Terraform provider is a plugin that enables Terraform to interact with APIs of cloud platforms (e.g., AWS, Azure) or other services, managing their resources through defined configurations.

##
#### 19.  Examples of a data resource in AWS?

     Answer: AMIs, aws_instance
             Please see:  https://registry.terraform.io/providers/hashicorp/aws/latest/docs/data-sources/instance

##
#### 20.  When you run your Terraform code on local, how does it get access to your AWS / GCP account?

     Answer: There are couple of ways, but one way is using variables:

              AWS_ACCESS_KEY_ID, AWS_SECRET_ACCESS_KEY 
              Environment Variables: Setting credentials via environment variables like AWS_PROFILE, GOOGLE_APPLICATION_CREDENTIALS.    
              Credential Files: Using ~/.aws/credentials or GCP's application_default_credentials.json.
              Instance Metadata: If running on an EC2 or GCP VM, Terraform can use the instance's IAM role or service account.
### 20.1.  What is data resource? 
    Answer: A data resource in Terraform allows you to query and reference existing resources or information from your infrastructure, without modifying them. It provides data to be used in your Terraform configurations.
#### 21. Can you mark a variable a "secret" such that it does not show up in logs etc?

    Answer: Yes from version 0.14. Yes, in Terraform, you can mark a variable as a "secret" by using the sensitive = true attribute. This prevents the variable's value from being displayed in logs or output.

##
#### 22. You made Terraform state using v 0.13. Can you modify it using version 0.12?

   Answer: NO. But, this is possible from 0.14 and up.


##
#### 23. Can Terraform use Bash environment variables?

   Answer: Yes, mostly. 
           Please see here:
           https://stackoverflow.com/questions/53330060/can-terraform-use-bash-environment-variables


##
#### 24. Can a module call another module?

   Answer: Yes,but it is not recommended


##
#### 25. Why do we need terraform.tfvars file?

   Answer: If you do not hard code variables, you can set values in terraform.tfvars files (different values per environment). This way, your code doesn't change and you can follow DRY principle.
##
#### 26. If you were to design terraform code for making AWS Security Groups, how would you do it?

  Answer: This one is bit complicated because Secuirty Groups are made up of Secrutiy Group Rules. So, there are two ways you can go about this:
          A. Made a module for SG Rules. A template would call that module N times to build a SG.
          B. 0.13 can take .csv file as input. You can use that to build SG as a List of Maps.


##
#### 27. Which folder caches module codes locally?

  Answer: .terraform


##
#### 28. What negative impact does it have if you remove .terraform folder?

    Answer: None. Terraform will simply download everything when you run terraform init command


##
#### 29. You have the same tf code. WIth the same code, you want to deploy to N environments (states) and be able switch between them. How?

     Answer: Use different .tfvars file for each environment



##
#### 30. How can you pass csv an argument to a module?

     Answer:  Use csvdecode function. That will read a csv file into a List of Maps. You can pass that directly to a module that accepts List of Maps (One input Variable)
           Please see here for more info on csvdecode:
           https://www.terraform.io/docs/language/functions/csvdecode.html

##
#### 31. Does Terraform have built-in functions?

  Answer: Yes. Many. e.g. csvdecode.
    String Functions: join, split, trimspace, replace
    Numeric Functions: min, max, abs
    Collection Functions: length, lookup, merge
    Date and Time Functions: timestamp, formatdate
    Encoding Functions: base64encode, base64decode



##
#### 32. What does "terraform refresh do"?

  Answer: terraform refresh updates the Terraform state file by syncing it with the real-world infrastructure. It re-fetches the current state of resources to ensure the state file matches the actual infrastructure, without making any changes to the resources.
          More info here:
          https://stackoverflow.com/questions/42628660/what-does-terraform-refresh-really-do


##
#### 33. You have added some new codes in the outputs.tf file (added more things to output). That is the only thing you have added. What command can you now run so that you get to see the results even though no change has happened in any other code or any resources in the cloud?

  Answer: terraform refresh


##
#### 34. What does "terraform taint" do?

  Answer: terraform taint marks a specific resource in the Terraform state as "tainted," which forces it to be destroyed and recreated during the next terraform apply. This is useful when you need to rebuild a resource without changing the overall infrastructure plan. This is a good way to force immutability.
# why we use taint command in terraform? 
It is  useful for forcing updates or resolving issues without modifying the configuration.
##
#### 35. How can you get a list of Terraform resources of a given folder with terraform code?

    Answer: terraform state list


##
### 36. Can you move terraform state from one place to another?

    Answer: Yes, you can move Terraform state using the terraform state mv command. It allows you to relocate resources within the state file or to a new location, such as switching backends (e.g., from local to remote storage).

##
#### 37. Can you remove JUST 1 item from your "state"? How?

    Answer: terraform state rm <resource_name>
    This removes the resource from the state file without deleting the actual resource, allowing Terraform to stop tracking it.

##
#### 38. Using Terraform , you have deployed 6 resources in AWS. However, a developer deleted one of them via AWS Console. Turns out that , that resource was not needed any way. How can you make your terraform code and terraform state sync up now?

    Answer: 1. terraform state rm resource_name &
            2. remove the relevant part of the code that creates the deleted resource
        No, terraform refresh updates the state with current resource data but does not remove resources that were manually deleted. You need terraform state rm to remove such resources from the state file. terraform plan then verifies the configuration and state consistency.

##
#### 39. Generally speaking, using --auto-approve is bad idea. Can you imagine a situation where , it may ok ?

    Answer: In development environments

##
#### 40. Examples of other data resources that are part of AWS provider:

    Answer:
      "aws_acm_certificate"
      "aws_api_gateway_account"
      "aws_codecommit_repository" on and on and on


##
#### 41. How does terraform get access to AWS?

    Answer: Using variable in aws provider section

    provider "aws" {
        access_key = "<AWS_ACCESS_KEY>"
        secret_key = "<AWS_SECRET_KEY>"
        region = "<AWS_REGION>"
    }


##
#### 42. How would switch between versions of Terraform?

    Answer:
      mac brew install tfenv
      tfenv (switches between many versions super easily.)
    To switch between Terraform versions, use a version manager like tfenv or tfswitch. Alternatively, manually download and install the desired version, and update your system's PATH variable to point to the new Terraform binary.
      There are other ways , as well (e.g. using containers and aliases)

##
#### 43. Scenario Question: Your terraform code, state and cloud resources are all sync.Now, you run: terraform state rm foo (foo is one of resources). What will happen now,if you run "terraform plan" ?

    Answer: After running terraform state rm foo, the resource foo is removed from the Terraform state file but remains in the cloud. Running terraform plan will show foo as a resource to be created, since Terraform no longer tracks it

##
#### 44. What is good command to make your terraform code presentable?

    Answer: terraform fmt

##
#### 45. What does terraform init command do?

    Answer: 
The terraform init command initializes a Terraform configuration directory by:

        Downloading and installing the required provider plugins.
        Setting up the backend for state management.
        Initializing the working directory for Terraform operations.
        It prepares the environment for subsequent commands.

##
#### 46. Is there a linting tool for Terraform?

    Answer: Yes. tflint (open source). tflint is a linting tool for Terraform. It checks your Terraform configuration files for common issues, best practices, and potential errors, helping to enforce coding standards and improve code quality before applying changes.

##
#### 47. Is there a tool that can look for security vulnerabilities in your terraform code?

    Answer: Yes. tfsec. tools like Checkov, TFSec, and Terraform Compliance scan Terraform code for security vulnerabilities and best practices. They analyze configurations for potential risks, misconfigurations, and policy violations, providing recommendations to enhance security and compliance.

##
#### 48. Does "providers" have versions too?

    Answer: Yes!

##
#### 49. How do you make sure your code is using a particular version of a provider?

    Answer: In the provider block, you set the version.

      Here is an example from Hashicorp web site:


    provider "aws" {
      version = "~> 1.0"
      access_key = "foo"
      secret_key = "bar"
      region     = "us-east-1"
    }


##
#### 50. How do you upgrade the version or providers and plugins and modules?


    Answer: Providers: Update the version constraint in the terraform block and run terraform init -upgrade.
Modules: Update the module version in the configuration and run terraform get -update.

##
#### 51. What is Interpolation ?

   Answer: Basically using variables.
           0.12 or below, you can use funny $ { } syntax.
           0.13 or above, much more user-friendly

##
#### 52. How can you make Lambda functions using Terraform?

   Answer:
       Lambda codes live in a folder called src and zip file is created on the fly by 
       terraform (when terraform apply runs) and terraform apply will also use the zip 
       file to deploy lambda function to AWS.
        Define the Lambda Function: Use the aws_lambda_function resource.
        Specify the Handler, Runtime, and Code: Provide the function’s entry point, runtime environment, and deployment package.
        Deploy: Apply the configuration with terraform apply.

##
#### 53. If you have 10 different .tf files in a folder, in what order does Terraform execute the code?

    Answer: Terraform processes all .tf files in a directory simultaneously, merging them into a single configuration. The order of execution for resources, modules, and providers is determined by dependency relationships, not by the file order. All files are read in parallel.

##
#### 54. You have made changes to your code. You did not run terraform plan. You now run terraform apply. What will happen?

    Answer: terraform will run terraform plan anyway before running terraform apply. Running terraform apply without running terraform plan will directly apply the changes from your Terraform configuration. It will attempt to modify your infrastructure according to the updated code, potentially leading to unintended or unreviewed changes.

##
#### 55. Besides terraform.tfvars file, which other files does terraform load for variable values?

    Answer: *.auto.tfvars

##
#### 56. You are building ec2 machines via terraform. However, you also have to install software and configurations on these ec2 machines. How can you do this using Terraform?

    Answer: There are multiple ways. One way is to use user-data scripts.To install software and configure EC2 instances with Terraform, use the user_data parameter in the aws_instance resource. Provide a shell script or cloud-init configuration to automate software installation and setup during instance launch.

##
#### 57. How to create ssh key using terraform:

    Answer:  Terraform can generate SSL/SSH private keys using the tls_private_key.To create an SSH key using Terraform, use the tls_private_key resource. It generates a new SSH key pair. You can then reference the private key for secure access to EC2 instances or other resources.

##
#### 58.  How can you do "if-then-else" logic in Terraform version 0.13 and above?
     
     Answer: (Directly from terraform pages:)

      A common use of conditional expressions is to define defaults to replace invalid values:
         var.a != "" ? var.a : "default-a"
         If var.a is an empty string then the result is "default-a", but otherwise it is the actual value of var.a.
        In Terraform 0.13 and above, "if-then-else" logic is implemented using the ternary operator. The syntax is condition ? true_value : false_value, where the condition is evaluated, returning true_value if true, or false_value if false.

##
#### 59. What is "Dynamic Block" in Terraform?

    Answer: It is kind of like a for loop.A Dynamic Block in Terraform allows you to generate multiple nested blocks within a resource or module based on a variable or condition. It's useful for creating repeated configurations dynamically, reducing code duplication.


##
#### 60. During terraform plan , a resource is successful  (i.e. there are no issues with terraform plan), but fails during provisioning (i.e. during "apply") , what happens to the resource in terraform state?

    Answer: It is marked as "tainted". 

##
#### 61. If a provider needs to download data, in which phase does it happen?

    Answer: terraform plan

##
#### 62. In "terraform" block , how to do you make sure that user is using terraform version 0.11 or above?

    Answer:

    terraform { required_version = "~> 0.11" } 

##
#### 63.  Do Plugins execute as separate processes?

     Answer: Yes.

##
#### 64. What is the difference between provideer and a plugin?

    Answer: It is kind of same, but not :-)
      Best answer is given here:
      https://stackoverflow.com/questions/63440271/understanding-terraform-provider-and-plugin
    A Terraform provider is a specific plugin that allows Terraform to interact with external APIs or cloud services, managing their resources. A plugin is a broader term, encompassing providers and provisioners, extending Terraform’s functionality by adding new capabilities.
##
#### 65. As best practice and code readibility, you should always have ___________ file?

    Answer: main.tf  (For the reader of your code, this is the entrypoint)

##
#### 66. You have code for resource A and resource B in your main.tf . However, you do not want resource B created until resource A
    is created.  How do you accomplish this?

    Answer: In the block for resource B, add a depends_on parameter.

##
#### 67. Can you use filter inside a data block?

    Answer: Yes.

##
#### 68. Which will tell terraform to look for all module source lines and retrieves the module codes and report errors if can't find them ?

    Answer: terraform get

##
#### 69. What is a terraform provisioner?

    Answer: (straight from Hashicorp documentation):

      Provisioners are used to execute scripts on a local or remote machine as part of resource creation or destruction. 
      Provisioners can be used to bootstrap a resource, cleanup before destroy, run configuration management, etc. 

##
#### 70. Examples of provisioners:

    Answer: local-exec , remote-exec

##
#### 71. What is a null resource?

    Answer: It is a thing runs through the resource life-cycle, but actually does nothing.
    A null resource in Terraform is a resource that doesn’t manage any specific infrastructure but can run provisioners or triggers. It's useful for running scripts or commands when certain conditions change, without managing an actual resource.

##
#### 72. If a null resource takes no action, what could possibly a use case for it?

    Answer: null resource has a magic parameter called "triggers". Any change in the triggers, makes the null-resource run its resource
            cycle again.


            Here is example from:  
            https://blog.logrocket.com/dirty-terraform-hacks/


            variable "config_path" {
              description = "path to a kubernetes config file"
            }
            variable "k8s_yaml" {
              description = "path to a kubernetes yaml file to apply"
            }

            resource "null_resource" "kubectl_apply" {
              triggers = {
                config_contents = filemd5(var.config_path)
                k8s_yaml_contents = filemd5(var.k8s_yaml)
              }

              provisioner "local-exec" {
                command = "kubectl apply --kubeconfig ${var.config_path} -f ${var.k8s_yaml}"
              }
            }


            In the above example, any changes to the contents of the Kubernetes config file or Kubernetes YAML will cause the command to rerun.

##
#### 73. You want to run terraform plan. However, you want to point to a non-default state file. How?

    Answer: -state=PATH



##
#### 74. True or False: Providers and provisioners are both provided via plugins.

    Answer: True


##
##### 75. Can terraform handle map type variable natively?

    Answer: Yes


##
#### 76.  A ________ type is a type that groups multiple values into a single value.


    Answer: Complex


##
#### 77.  Launch interactive console via which command?

    Answer: terraform console


##
#### 78.  If you want 2 identical aws provider sections, then you have use :

     Answer:  alias keyword on the 2nd one.


##
##### 79. Can plugins vary folder by folder?


    Answer: plugins for each terraform code folder are independent of each other.


##
#### 80. You have file foo.tf with 5 resources defined in it. You also bar.tf with another 5 resources defined in it.  If you concatenate the 2 files and deleted foo.tf bar.tf files, what would be the impact?

    Answer: nothing

##
##### 81. State file is always encrypted at rest. True or False:

    Answer: False

##
#### 83. If you have chosen S3 as your backend for state files. What's easiest way to make sure state file(s) are encrypted at rest?


    Answer: Just make the bucket encrypted

##
#### 84. _________ command creates a visual graph of Terraform resources:


    Answer: terraform graph

##
#### 85. What allows terraform users to apply policy as code to enforce standardized configurations for resources being deployed?


    Answer: Sentinel

##
#### 86. When terraform init downloads plugins, where does it save it?


    Answer: .terraform.d folder

##
##### 87. Where is local copy of the modules saved?


    Answer: .terraform folder

##
#### 88. Which file keeps a local copy of the "state" (if remote state is not used) ?


    Answer: terraform.tfstate


##
#### 89. Have you noticed any change with respect to error handling beginning version 0.13?


    Answer: Yes. It is LOT better. For example, it points to file name and line numbers very accurately (where the problem is). 
            Also, error messages , in general, are significantly clearer.

##
#### 90. Every Terraform resource has a meta-parameter you can use for iteration called index. what is the puspose of it?


    Answer: In Terraform, the count.index meta-parameter is used for iteration when you define a resource with the count parameter. It allows you to access the current index in the iteration, enabling you to differentiate between multiple instances of a resource. This is particularly useful when creating multiple similar resources, such as virtual machines or network interfaces, where each instance needs a unique configuration or name.

##
#### 91. Scenario Question: You wrote some terraform code. You ran plan and apply and resources got created. Then, you ran terraform destroy and resources got destroyed. What happens if you now run "terraform plan" ?


    Answer: It will say that it needs to re-create the same resources again.


##
##### 92. Is Terraform idempotent?

   Answer: Yes

##
#### 93. What does "root module" mean and what does it do?

    Answer: It's the folder that may or may not call other child modules , but no other code calls this folder as a module.
    The "root module" in Terraform is the top-level module that contains the main configuration files, typically main.tf, variables.tf, and outputs.tf. It serves as the entry point for Terraform, orchestrating the creation and management of resources.
##
##### 94. Why is root module called a "module" when its just a folder?


    Answer: The root module is called a "module" because, like any other Terraform module, it consists of a set of configuration files that define resources and their relationships. Terraform treats it as a module, even though it's the starting point.


##
#### 95.  When you run "terraform plan", if there are no syntax error, you will get what at the bottom of the output?


     Answer: Number of sources to be created, modified, and destroyed

##
#### 96. Is there way to validate your terraform code syntax, before running terraform plan?


    Answer: Yes. "terraform validate"

##
#### 97. Is there way to automatically create a Readme.md file based on your terraform code?


    Answer:  terraform-docs markdown . >> README.md
      Yes, tools like terraform-docs can automatically generate a README.md file based on your Terraform code. These tools extract information from your configuration, such as inputs, outputs, and modules, and format it into a Markdown file.
    
##
#### 98. Persistence data stored in state for a particular environment is called?

    
    Answer: workspace

##
#### 99. Which workspace do you work in by default?


    Answer: default


##
#### 100. How do you know which workspaces you have in the first place?


    Answer: terraform workspace list

##
#### 101. What if you do not want terraform apply to ask for your permission before deploying resources?

    Answer: terraform apply --auto-approve

# # ================== Scanerio Based Questions =================================

# Scenario -1 : You need to create multiple identical AWS EC2 instances. How would you do this in Terraform?

        Answer: Use the count parameter to define the number of instances and count.index to 
        customize each instance if necessary.
##
#### Scenario: You need to manage resources across multiple AWS accounts. How do you achieve this in Terraform?

        Answer: Use multiple provider configurations, each with different AWS credentials,
         and define alias providers to target specific accounts.
##
#### Scenario: Your Terraform apply failed due to a dependency issue. How do you resolve it?

        Answer: Use the depends_on parameter to explicitly define dependencies between resources
    or review the configuration for missing dependencies.
##
#### Scenario: You want to create different environments (e.g., dev, prod) with minimal code duplication. How do you approach this?

        Answer: Use workspaces or separate environment-specific .tfvars files combined with a single module to manage environments.
##
#### Scenario: An S3 bucket in your Terraform configuration was deleted manually. What happens in the next terraform apply?

        Answer: Terraform will recognize the bucket as missing and recreate it during the next 
        terraform apply unless it's removed from the configuration.
##
#### Scenario: You need to manage a resource that Terraform doesn’t have a provider for. How can you manage it?

        Answer: Use a null_resource with local-exec or remote-exec provisioners to run scripts that manage the resource outside of Terraform.
##
#### Scenario: You want to pass sensitive data, like passwords, to Terraform modules. How do you do it securely?

        Answer: Use Terraform's sensitive variable type and store sensitive data in environment variables or encrypted backends like AWS Secrets Manager.
##
#### Scenario: After making changes, you want to preview what will happen without applying the changes. How do you do this?

        Answer: Run terraform plan to preview changes without applying them.
##
#### Scenario: You need to remove a resource from the state but not delete it from the actual infrastructure. How do you proceed?

        Answer: Use terraform state rm <resource> to remove it from the state file without affecting the actual resource.
##
#### Scenario: You need to run a script only on the first creation of an EC2 instance. How would you configure this?

        Answer: Use the create lifecycle hook within a provisioner block to ensure the script runs only on resource creation.
##
#### Scenario: How do you manage multiple versions of a module for different teams?

        Answer: To manage multiple versions of a module for different teams, use versioning in the module's source definition by specifying the version attribute. Store the modules in a central registry, like Terraform Registry, or a private Git repository.
##
#### Scenario: Your team needs to audit infrastructure changes. How can Terraform help?

        Answer: Use a remote backend with versioning and logging enabled, such as an S3 backend with versioning, to track and audit changes.
##
#### Scenario: How do you handle situations where two teams are working on the same infrastructure simultaneously?

        nswer: Use state locking with a remote backend (e.g., S3 with DynamoDB for locking) to prevent simultaneous writes to the state file.
##
#### Scenario: You want to import existing infrastructure into Terraform. How do you do this?

        Answer: Use the terraform import command to bring existing resources under Terraform management.
##
#### Scenario: You notice that applying changes is taking longer due to unnecessary re-creations. How do you troubleshoot?

Answer: Review the terraform plan output for changes triggered by attributes that shouldn't change. Use lifecycle settings like ignore_changes to prevent unnecessary updates.
##
#### Scenario: A new feature in your Terraform module should be optional. How do you implement this?

Answer: Use conditional expressions or default values in the module’s input variables to make the feature optional.
##
#### Scenario: You’re asked to split a large Terraform configuration into manageable parts. How do you do this?

Answer: Break the configuration into modules, each responsible for a specific part of the infrastructure, and use them in the root module.
##
#### Scenario: How do you roll back to a previous state if a deployment goes wrong?

Answer: Restore a previous state file manually or use terraform apply with an older state file if backed up, ensuring resources are consistent with the desired state.
##
#### Scenario: You need to manage the same Terraform configuration for different cloud providers. How do you do this?

Answer: Use provider abstraction by creating separate modules for each cloud provider, with shared logic in the root module and provider-specific implementations in submodules.
##
#### Scenario: You need to provision resources based on a complex condition involving multiple variables. How do you handle this?

Answer: Use count, for_each, and conditionals to manage resources dynamically, or leverage the dynamic block for more complex scenarios.



















             

                                         Terraform Interview Questions

                                                Mamun Rashid

                                        https://www.linkedin.com/in/mamunrashid/


                                        Edited by: Sohail Abbas

                                        Portfolio: https://github.com/Sohail-DevOps
                                        Linkedin : https://www.linkedin.com/in/sohailabbasdevops/

