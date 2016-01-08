This is based on using a free sms service called textbelt.com/text


What to do to get this working:

Note: These instructions are debian based.


##Step 1##
Download send_textbelt_sms to /usr/local/nagios/libexec

chmod +x send_textbelt_sms 

replace REPLACEWITHPHONENUMBER with your phone number e.g. 8185551213



##Step 2##

Add the following lines to commands.cfg

sudo nano /usr/local/nagios/etc/objects/commands.cfg




# 'notify-service-by-sms' command definition
define command{
        command_name notify-service-by-sms
        command_line /usr/local/nagios/libexec/send_textbelt_sms "--Nagios Service Notification-- Host: $HOSTNAME$, Service: $SERVICEDESC$, Description: $SERVICESTATE$, Time: $LONGDATETIME$"
}

# 'notify-host-by-sms' command definition
define command{
        command_name notify-host-by-sms
        command_line /usr/local/nagios/libexec/send_textbelt_sms "--Nagios Host Notification-- Host: $HOSTNAME$, State: $HOSTSTATE$, Time: $LONGDATETIME$"
}


##Step 3 ##
Modify contacts.cfg
sudo nano /usr/local/nagios/etc/objects/contacts.cfg

Modify your contact definition to look like the following:


define contact{
        contact_name                    nagiosadmin             ; Short name of user
        use                             generic-contact         ; Inherit default values from generic-contact template (defined above)
        alias                           Nagios Admin            ; Full name of user
        service_notification_commands   notify-service-by-sms    ;sms notices
        host_notification_commands      notify-host-by-sms      ;sms notices
        email                           youremailaddy@xyz.com        ; <<***** CHANGE THIS TO YOUR EMAIL ADDRESS ******
        }


Note:

The lines added to contacts.cfg are:


                        
        service_notification_commands   notify-service-by-sms    ;sms notices
        host_notification_commands      notify-host-by-sms      ;sms notices


## Step 4 ##
Reload Nagios Configs

sudo /etc/init.d/nagios reload







