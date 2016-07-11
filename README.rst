OpenStack-Ansible Horizon
#########################

This Ansible role installs and configures OpenStack Horizon served by the
Apache webserver. Horizon is configured to use Galera for session caching and
memcached for other caching.

Default Variables
=================

.. literalinclude:: ../../defaults/main.yml
   :language: yaml
   :start-after: under the License.


Required Variables
==================

This list is not exhaustive at present. See role internals for further
details.

.. code-block:: yaml

      horizon_ssl_protocol: "ALL -SSLv2 -SSLv3"
      horizon_ssl_cipher_suite: "ECDH+AESGCM:DH+AESGCM:ECDH+AES256:DH+AES256:ECDH+AES128:DH+AES:ECDH+3DES:DH+3DES:RSA+AESGCM:RSA+AES:RSA+3DES:!aNULL:!MD5:!DSS"
      horizon_galera_address: 10.100.100.101
      horizon_container_mysql_password: "SuperSecrete"
      horizon_secret_key: "SuperSecreteHorizonKey"


Example Playbook
================

.. code-block:: yaml

    - name: Installation and setup of horizon
      hosts: horizon_all
      user: root
      roles:
        - { role: "os_horizon", tags: [ "os-horizon" ] }
      vars:
        galera_client_drop_config_file: false
        external_lb_vip_address: 10.100.100.101
        internal_lb_vip_address: 10.100.100.101
        horizon_galera_address: 10.100.100.101
        horizon_container_mysql_password: "SuperSecrete"
        horizon_secret_key: "SuperSecreteHorizonKey"
        horizon_external_ssl: true
        horizon_ssl_protocol: "ALL -SSLv2 -SSLv3"
        horizon_ssl_cipher_suite: "ECDH+AESGCM:DH+AESGCM:ECDH+AES256:DH+AES256:ECDH+AES128:DH+AES:ECDH+3DES:DH+3DES:RSA+AESGCM:RSA+AES:RSA+3DES:!aNULL:!MD5:!DSS"
        galera_root_password: "secrete"
        rabbitmq_servers: 10.100.100.101
        rabbitmq_use_ssl: false
        rabbitmq_port: 5671
        keystone_admin_user_name: admin
        keystone_auth_admin_password: "SuperSecretePassword"
        keystone_admin_tenant_name: admin
        keystone_service_adminuri_insecure: false
        keystone_service_internaluri_insecure: false
        keystone_service_internaluri: "http://{{ internal_lb_vip_address }}:5000"
        keystone_service_internalurl: "{{ keystone_service_internaluri }}/v3"
        keystone_service_adminuri: "http://{{ internal_lb_vip_address }}:35357"
        keystone_service_adminurl: "{{ keystone_service_adminuri }}/v3"
        openrc_os_password: "{{ keystone_auth_admin_password }}"
        openrc_os_domain_name: "Default"
        memcached_servers: 10.100.100.101
        memcached_encryption_key: "secrete"

Tags
====

This role supports two tags: ``horizon-install`` and ``horizon-config``

The ``horizon-install`` tag can be used to install and upgrade.

The ``horizon-config`` tag can be used to manage configuration.
