# WSO2 API Management Ansible scriptss

This repository contains the Ansible scripts for installing and configuring WSO2 API Management.

## Supported Operating Systems

- Ubuntu 16.04 or higher
- CentOS 7

## Supported Ansible Versions

- Ansible 2.8.0

## Directory Structure
```
.
├── dev
│   ├── group_vars
│   │   ├── apim-analytics.yml
│   │   ├── apim-is-as-km.yml
│   │   └── apim.yml
│   ├── host_vars
│   │   ├── apim_1.yml
│   │   ├── apim-analytics-dashboard_1.yml
│   │   ├── apim-analytics-worker_1.yml
│   │   ├── apim-ex-gateway_1.yml
│   │   ├── apim-gateway_1.yml
│   │   ├── apim-is-as-km_1.yml
│   │   ├── apim-km_1.yml
│   │   ├── apim-publisher_1.yml
│   │   ├── apim-store_1.yml
│   │   └── apim-tm_1.yml
│   └── inventory
├── docs
│   ├── images
│   │   ├── API-M-single-node-deployment.png
│   │   ├── P-H-1.png
│   │   ├── P-H-2.png
│   │   ├── P-H-3.png
│   │   ├── P-M-1.png
│   │   └── P-S-1.png
│   ├── Pattern_1.md
│   ├── Pattern_2.md
│   ├── Pattern_3.md
│   ├── Pattern_4.md
│   └── Pattern_5.md
├── files
│   ├── lib
│   │   ├── amazon-corretto-8.202.08.2-linux-x64.tar.gz
│   │   └── mysql-connector-java-5.1.47-bin.jar
│   └── packs
│       ├── update.sh
│       ├── wso2am-2.6.0.zip
│       ├── wso2am-analytics-2.6.0.zip
│       └── wso2is-km-5.7.0.zip
├── issue_template.md
├── LICENSE
├── pull_request_template.md
├── README.md
├── roles
│   ├── apim
│   │   ├── tasks
│   │   └── templates
│   ├── apim-analytics-dashboard
│   │   ├── tasks
│   │   └── templates
│   ├── apim-analytics-worker
│   │   ├── tasks
│   │   └── templates
│   ├── apim-ex-gateway
│   │   ├── tasks
│   │   └── templates
│   ├── apim-gateway
│   │   ├── tasks
│   │   └── templates
│   ├── apim-is-as-km
│   │   ├── tasks
│   │   └── templates
│   ├── apim-km
│   │   ├── tasks
│   │   └── templates
│   ├── apim-publisher
│   │   ├── tasks
│   │   └── templates
│   ├── apim-store
│   │   ├── tasks
│   │   └── templates
│   ├── apim-tm
│   │   ├── tasks
│   │   └── templates
│   └── common
│       └── tasks
└── site.yml

```

## Packs to be Copied

Copy the following files to `files/packs` directory.

1. [WSO2 API Manager 2.6.0 package](https://wso2.com/api-management/install/)
2. [WSO2 API Manager Analytics 2.6.0 package](https://wso2.com/api-management/install/analytics/)
3. [WSO2 API Manager Identity Server as Key Manager 5.7.0 package](https://wso2.com/api-management/install/key-manager/)

Copy the following files to `files/lib` directory.

1. [MySQL Connector/J](https://dev.mysql.com/downloads/connector/j/5.1.html)
2. [Amazon Coretto for Linux x64 JDK](https://docs.aws.amazon.com/corretto/latest/corretto-8-ug/downloads-list.html)

## Running WSO2 API Management Ansible scripts

### 1. Run the existing scripts without customization
The existing Ansible scripts contain the configurations to set-up a single node WSO2 API Manager pattern. In order to deploy the pattern, you need to replace the `[ip_address]` and `[ssh_user]` given in the `inventory` file under `dev` folder by the IP of the location where you need to host the API Manager. An example is given below.
```
[is]
wso2am ansible_host=172.28.128.4 ansible_user=vagrant
```

Run the following command to run the scripts.

`ansible-playbook -i dev site.yml`

If you need to alter the configurations given, please change the parameterized values in the yaml files under `group_vars` and `host_vars`.

### 2. Customize the WSO2 Ansible scripts

The templates that are used by the Ansible scripts are in j2 format in-order to enable parameterization.

The `axis2.xml.j2` file is added under `roles/wso2am/templates/carbon-home/repositoy/conf/axis2/`, in order to enable customizations. You can add any other customizations to `custom.yml` under tasks of each role as well.

#### Step 1
Uncomment the following line in `main.yml` under the role you want to customize.
```
- import_tasks: custom.yml
```

#### Step 2
Add the configurations to the `custom.yml`. A sample is given below.

```
- name: "Copy custom file"
  template:
    src: path/to/example/file/example.xml.j2
    dest: destination/example.xml.j2
  when: "(inventory_hostname in groups['am'])"
```

Follow the steps mentioned under `docs` directory to customize/create new Ansible scripts and deploy the recommended patterns.
s
