# OpenLDAP Installation

sudo apt-get install slapd ldap-utils
sudo dpkg-reconfigure slapd
DNS Domain Name: testldap.com
Org name: testldap
Admin Password: admin
sudo ufw allow ldap

# OpenLDAP GUI Installation
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
http://<publicip>/phpldapadmin

Login DN: cn=admin,dc=testldap,dc=com
password: admin

If you are not able to login due to some issue pls install the below hotfix.

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