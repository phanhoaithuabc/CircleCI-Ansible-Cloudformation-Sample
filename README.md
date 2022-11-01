# hello-world to Configuration Management with Ansible

```bash
├── ansible.cfg                         ### Config and Deployment
├── bucket.yml                          ### Promote to Production
├── cloudfront.yml
├── error.html
├── index.html
├── inventory.txt                       ### Exercise 3: Ansible to Install Apache and Copy File
├── main1.yml                           ### Classroom Demo: Design an Ansible  
├── main2-print-hello-world.yml         ### Exercise 1: Ansible to Print Hello World 
├── main3-install-apache-copy-file.yml  ### Exercise 2: Ansible to Install Apache and Copy File
├── main4-web-server-on-EC2.yml         ### Exercise 3: Ansible to setting web server in an EC2
├── roles
│   ├── apache                          ### Exercise 2: Ansible to Install Apache and Copy File
│   │   ├── files
│   │   │   └── index.html
│   │   └── tasks
│   │       └── main.yml
│   ├── configure-server                ### Classroom Demo: Design an Ansible  
│   │   └── tasks
│   │       └── main.yml
│   ├── prepare                         ### Exercise 2: Ansible to Install Apache and Copy File
│   │   └── tasks
│   │       └── main.yml
│   ├── print                           ### Exercise 1: Ansible to Print Hello World 
│   │   └── tasks
│   │       └── main.yml
│   └── setup                           ### Exercise 3: Ansible to setting web server in an EC2
│       ├── files
│       │   └── index.js
│       └── tasks
│           └── main.yml
└── template.yml                        ### Exercise: Infrastructure Creation
└── .circleci
    └── config.yml                      ### CircleCI exercises
```

> **Note**: When you attempt any CircleCI exercise, you should comment out the Jobs and Workflows from other exercises. This will ensure that you do not end up creating unnecessary EC2, S3, CloudFront resources in your AWS console. 

### 1. Exercise: Ansible  to Print Hello World
Files relevant for this exercise are:
```bash
├── main2-print-hello-world.yml
└── roles
    └── print
        └── tasks
            └── main.yml

Run your using the command: ansible-playbook main2-print-hello-world.yml
```

### 2. Exercise: Ansible to Install Apache and Copy File
Files relevant for this exercise are:
```bash
├── main3-install-apache-copy-file.yml
└── roles
    └── apache
        ├── files
        │   └── index.js
        └── tasks
            └── main.yml

Run your using the command: ansible-playbook main3-install-apache-copy-file.yml
```

### 3. Exercise: Ansible to setting web server in an EC2
**Prerequisite**: 
- A linux EC2 instance with port 3000 open for the inbound access. 
- Public IP address of an EC2 instance in your AWS account.
- A key pair to connect your EC2 instance

Files relevant for this exercise are:
```bash
├── main4-web-server-on-EC2.yml
└── roles
    └── setup
        ├── files
        │   └── index.js
        └── tasks
            └── main.yml

# Assuming the keypair.pem and inventory files are present in the current directory, run this command:
ansible-playbook main4-web-server-on-EC2.yml -i inventory --private-key keypair.pem
```

### 4. Exercise: Infrastructure Creation => Config and Deployment => Smoke Testing => Rollback
Files relevant for this exercise are:
```bash
├── EC2-w-SG-Cloudformation.yml # Change the KeyName property value, as applicable to you
├── ansible.cfg            # Disables host_key_checking 
├── inventory              # Or it could be inventory.txt
├── main.yml               # Playbook file from the Exercise: Remote Control Using Ansible
└── roles
│   └── setup
│       ├── files
│       │   └── index.js
│       └── tasks
│           └── main.yml
└── .circleci
    └─── config.yml        # Should have the create_infrastructure Job
```

### 5. Exercise: Promote to Production
**Prerequisite**: 
- An S3 bucket (say `mybucket644752792305`) containing a sample `index.html` file created manually in your AWS console. 
- Enable the Static website hosting in that bucket.
- Use the command below to create a CloudFront Distribution that will connect to the existing bucket. 
```bash
 aws cloudformation deploy \
 --template-file cloudfront.yml \
 --stack-name production-distro \
 --parameter-overrides PipelineID="mybucket644752792305"
 
 # `--parameter-overrides` specify a value that override parameter value in the cloudfront.yml template file. Assuming that the `PipelineID` parameter represents the bucket ID. 
 ```
Files relevant for this exercise are:
```bash  
├── bucket.yml          # Creates a new bucket and bucket policy.
├── cloudfront.yml      # Creates a Cloudfront Distribution that will connect to the existing ^ bucket.
├── error.html
├── index.html
└── .circleci
    └── config.yml      # 4 Jobs: create_and_deploy_front_end, get_last_deployment_id, promote_to_production, and clean_up_old_front_end
```
