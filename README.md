ASBRL-IAM
=========

Create a CloudFormation Template to Deploy a IAM Stack

Requirements
------------

None


Role Variables
--------------

- TEMPLATE_DEST: (String)
- TAGS:
    - RELEASE: (String)
    - ENVIRONMENT_TYPE: (String)
- ROLES: (List)
    - NAME: (String)
    - OUTPUT: True (Default)
    - ASSUME_ROLE_PRINCIPAL: (String)
    - POLICIES: (List)
    - MANAGEMENT_POLICIES: (List)
- INSTANCE_PROFILES: (List)
    - NAME: (String)
    - OUTPUT: True (Default)
    - ROLENAME: (String)
- SECURITYGROUPS: (List)
    - NAME: (String)
    - OUTPUT: True (Default)
    - DESCRIPTION: (String)
    - INGRESS: (List)


Dependencies
------------

None

Example Playbook
----------------

        - name: Generate {{EnvironmentType}} cf-parameters
          include_role:
            name: asbrl-ssm-parameterstore
          vars:
            TEMPLATE_DEST: ./artifacts/{{EnvironmentType}}/cf-rabbitstack-security.yml
            TAGS:
              RELEASE: "{{Release}}"
              ENVIRONMENT_TYPE: "{{EnvironmentType}}"
            ROLES:
            - NAME: node
              OUTPUT: false
              ASSUME_ROLE_PRINCIPAL: ec2.amazonaws.com
              POLICIES:
              - NAME: node
                DOCUMENT:
                  Version: "2012-10-17"
                  Statement:
                  - Effect: "Allow"
                    Action: 
                      - "ec2:DescribeInstances"
                    Resource: "*"
              MANAGEMENT_POLICIES:
              - arn:aws:iam::aws:policy/CloudWatchAgentServerPolicy
            INSTANCE_PROFILES:
            - NAME: node
              ROLENAME: node
            SECURITYGROUPS:
            - NAME: ec2internal
              DESCRIPTION: Security Group for internal communication between nodes. SHOULD NOT BE MODIFIED
              INGRESS: 
              - PROTOCOL: tcp
                FROM: 4369
                TO: 35682
                CIDRIP: 10.0.0.0/8
              - PROTOCOL: tcp
                FROM: 4369
                TO: 35682
                CIDRIP: 172.16.0.0/12
              - PROTOCOL: tcp
                FROM: 4369
                TO: 35682
                CIDRIP: 192.168.0.0/16
            
License
-------

BSD

Author Information
------------------

Moegui.com
