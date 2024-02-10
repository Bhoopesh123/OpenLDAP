# 1. OpenLDAP Installation
Below steps will help in installing OPEN LDAP Server on Machine.

    sudo apt-get install slapd ldap-utils
    sudo dpkg-reconfigure slapd
    DNS Domain Name: testldap.com
    Org name: testldap
    Admin Password: admin
    sudo ufw allow ldap

# 2. OpenLDAP GUI Installation
Below steps will help in installing GUI of OPEN LDAP Server on Machine.

    sudo apt-get install phpldapadmin
    sudo nano /etc/phpldapadmin/config.php

    Ctrl +W Search th keyword: setValue(
        $servers->setValue('server','name','My TESTLDAP Server');
    Give your domain name.
        $servers->setValue('server','base',array('dc=testldap,dc=com'));
    ‘Hide_template_warning’ should be true.
        $config->custom->appearance['hide_template_warning'] = true;
    Comment out
        #  $servers->setValue('login','bind_id','cn=admin,dc=example,dc=com');

Now open Browser and give 
    http://publicip/phpldapadmin

    Login DN: cn=admin,dc=testldap,dc=com
    password: admin

If you are not able to login due to some login issue pls install the below hotfix for ubuntu machine.

    wget http://archive.ubuntu.com/ubuntu/pool/universe/p/phpldapadmin/phpldapadmin_1.2.6.3-0.3_all.deb
    dpkg -i phpldapadmin_1.2.6.3-0.3_all.deb
    sudo apt-get -f install

Create OU: groups 

create group: admins 

    Create user: bhoopesh 
    Once a User is created, need to attach it to the “Group”. 
    click on “Group” and Select “Add new attribute” then select your users and choose “MemberUID” 

create group: editors 

    Create user: john 
    Once a User is created, need to attach it to the “Group”. 
    click on “Group” and Select “Add new attribute” then select your users and choose “MemberUID” 

create group: viewers 

    Create user: sam 
    Once a User is created, need to attach it to the “Group”. 
    click on “Group” and Select “Add new attribute” then select your users and choose “MemberUID” 

# 3. Install Grafana on Machine.

    sudo apt-get install -y apt-transport-https software-properties-common
    sudo wget -q -O /usr/share/keyrings/grafana.key https://apt.grafana.com/gpg.key
    echo "deb [signed-by=/usr/share/keyrings/grafana.key] https://apt.grafana.com stable main" | sudo tee -a /etc/apt/sources.list.d/grafana.list
    sudo apt-get update

    sudo apt-get install grafana

    sudo systemctl daemon-reload
    sudo systemctl enable grafana-server.service
    sudo systemctl start grafana-server

    Access the Grafana web interface with default login and password
    http://<publicip>:3000

    login: admin
    password: password

    ## If admin password doesn't work then perform below steps:
    sudo apt-get install sqlite3
    sudo sqlite3 /var/lib/grafana/grafana.db
    update user set password = '59acf18b94d7eb0694c61e60ce44c110c7a683ac6a8f09580d626f90f4a242000746579358d77dd9e570e83fa24faa88a8a6', salt = 'F3FAxVm33R' where login = 'admin';
    Now, you could log in your Grafana web interface using 
    username: admin 
    password: admin
    ########


# 4. Grafana OpenLDAP Configuration and Integration

    cd /etc/grafana

    Enable Grafana.ini file as below:
    #################################### Auth LDAP ##########################
    [auth.ldap]
    enabled = true
    config_file = /etc/grafana/ldap.toml
    allow_sign_up = true
    # prevent synchronizing ldap users organization roles
    ;skip_org_role_sync = false

    # LDAP background sync (Enterprise only)
    # At 1 am every day
    ;sync_cron = "0 1 * * *"
    ;active_sync_enabled = true