[![CircleCI](https://circleci.com/gh/miyuk/ansible_bigip_certificate.svg?style=svg)](https://circleci.com/gh/miyuk/ansible_bigip_certificate)

# BIG-IP Certificate

This is an Ansible role to Install Certificate and Private Key for BIG-IP device.

This module can use transport type `rest` only.

## Requirements

This role has some dependencies to play.

- f5-sdk
- bigsuds

## Role Variables

| Variable | Description | Default |
| --- | --- | --- |
| `bigip_rest_provider` | bigip login credential for rest api use. | object |
| `bigip_src_crt_path` | source certificate path. this file is on server to play this role. | "{{ certificate_directory }}/{{ certificate_crt_filename }}" |
| `bigip_src_key_path` | source private key path. this file is on server to play this role. | "{{ certificate_directory }}/{{ certificate_crt_filename }}" |
| `bigip_partition` | bigip partition. | Common |
| `bigip_dest_crt_path` | destination certificate path. this file usually under `/config/ssl/ssl.crt` directory. | "/config/ssl/ssl.crt/{{ bigip_dest_crt_name }}" |
| `bigip_dest_key_path` | destination private key path. this file usually under `/config/ssl/ssl.key` directory. | "/config/ssl/ssl.key/{{ bigip_dest_key_name }}" |
| `bigip_dest_crt_name` | BIG-IP certificate name. | server |
| `bigip_dest_ssl_profile_name` | BIG-IP Client SSL Profile name. | server |

## Install

this module name is `bigip_certificate`. So, you try to install below command under Ansible Playbook folder.

```bash
cd roles
git clone git@github.com:miyuk/ansible_bigip_certificate.git bigip_certificate
```

## Example Playbook

Install certificate from `<playbook directory>/certs`.

```yaml
---
- hosts: localhost
  connection: local
  vars:
    bigip_rest_provider:
      server: 192.168.1.1
      user: admin
      password: admin
      server_port: 443
      transport: rest
      validate_certs: no
    bigip_src_crt_path: "./certs/{{ certificate_crt_filename }}"
    bigip_src_key_path: "./certs/{{ certificate_crt_filename }}"
    bigip_partition: Common
    bigip_dest_crt_path: "/config/ssl/ssl.crt/{{ bigip_dest_crt_name }}"
    bigip_dest_key_path: "/config/ssl/ssl.key/{{ bigip_dest_key_name }}"
    bigip_dest_crt_name: server
    bigip_dest_ssl_profile_name: server
  roles:
    - bigip_certificate
```