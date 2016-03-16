# drupsible-samba
Drupsible role for installing Samba inside a Vagrant guest running on a Windows host. Its main purpose is to overcome the known limitations and pitfalls of the NFS sync'ed folders of Vagrant.

This role installs and configures smbd and nmbd services, and creates two shares:
- app shared folder
- vagrant home shared folder

## Work your Drupal code
You will notice a new "LOCAL" server in your network when the Vagrant VM is up. This server will have these two shared folders inside, for example: 
- local.mygreatwebsite.com app
- local.mygreatwebsite.com vagrant home

The shares are named after the ``app_webhost`` and ``webdomain`` configured in the inventory file ``ansible/inventory/<app_name>-local``

The app share is the important one, but you still have access to the home folder in case you want to make changes to the Vagrantfile or ansible/requirements.yml file.

Open ``local.<webdomain>.com app`` and you will see a list of folders, something like this:
- logs
- public_html
- public\_html.20160114_1138
- public_html.bak
- public\_html.20160113_1452

public\_html and public\_html.bak are symbolic links to the matching public\_html.<date>_<time> folders. 

public\_html contains your Drupal codebase, to be edited with your favorite editor or sync'ed with your IDE workspace.

## Logs available!
The logs folder contains:
- access.log (Apache access log)
- drupal.log (Drupal syslog events)
- error.log (Apache error log

You can always disable syslog events in the ``ansible/inventory/group_vars/<app_name>-local/deploy.yml`` syslog parameter (yes/no).
