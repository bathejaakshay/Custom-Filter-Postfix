# Custom-Filter-Postfix

Creating a custom filter in Postfix is very easy!!  

Our use case: 

We want to run a python file whenever a mail is transferred by out MTA postfix server. This Python file will send alerts based on the sender recipient etc.  

## Step 1: Create alert.py in your sudo user's home directory.
```
#!/usr/bin/env python3
import sys

sender = sys.argv[1]
recipient = sys.argv[2]

# Perform blacklist checks and send alerts based on sender and recipient
# ...
```

## Step 2: Add custom_filter variable in postfix/main.cf

```
# Custom content filter
content_filter = myfilter

# Custom content filter configuration
myfilter_destination_recipient_limit = 1
myfilter_directory = /home/sudo_user_name
myfilter_command = /home/sudo_user_name/alert.py
```

## Step 3: Edit the Postfix master configuration file /etc/postfix/master.cf and add the following lines at the end (Replace variable sudo_user_name with actual name)

```
# Custom content filter
myfilter unix - n n - - pipe
    flags=Rq user=sudo_user_name argv=/opt/scripts/blacklist_alert.py ${sender} ${recipient}
```
## Step 4: Reload the Postfix configuration
```
sudo postfix reload
```
