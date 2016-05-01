troykinsella.aws-infrastructure
===============================

AWS infrastructure simplified.

Ideas taken from telusdigital.aws-infrastructure.

Role Variables
--------------


Example Playbook
----------------

Including an example of how to use your role (for instance, with variables passed in as parameters) is always nice for users too:

    - hosts: servers

      vars:
        aws_project: foo
        aws_subnet_prefix: "10.0"
        aws_region: us-west-2
        aws_key_pair_name: admin
        aws_environment_name: dev
        aws_subnets:
          - cidr: "{{ aws_subnet_prefix }}.0.0/27"
            az: "{{ aws_region }}a"
            resource_tags:
              'Project': "{{ aws_project }}"
              'Role':    'application'
              'Name':    "{{ aws_project }}-application-a"
        aws_security_group:
          inbound-web:
            description: "Inbound Web"
            rules:
            - { proto: tcp, from_port: 80,  to_port: 80,  cidr_ip: 0.0.0.0/0 }
            - { proto: tcp, from_port: 443, to_port: 443, cidr_ip: 0.0.0.0/0 }
          application:
            description: "Application Nodes"
            rules:
            - { proto: tcp, from_port: 80,  to_port: 80,  group_id: "{{ security_group_id['inbound-web']|default(-1) }}" }
            - { proto: tcp, from_port: 443, to_port: 443, group_id: "{{ security_group_id['inbound-web']|default(-1) }}" }
        aws_ec2_instances:
          - { system_role: "application", instance_type: "m1.small", assign_public_ip: "yes" }

      roles:
        - role: troykinsella.aws-infrastructure

License
-------

MIT
