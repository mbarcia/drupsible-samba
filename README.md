# drupsible-samba
Drupsible role for installing Samba inside a Vagrant guest running on a Windows host. Its main purpose is to overcome the known limitations and pitfalls of the vboxsf and NFS sync'ed folders of Vagrant.

This role installs and configures smbd and nmbd services, and creates 3 shares:
- app
- home
- roles

All the shares allow guest read/write access, and new files/dirs are created with the proper permissions inside the guest.

## Work your Drupal code
* On Windows Explorer, you will notice a new "LOCAL" server in your network when the Vagrant VM is up.
* On OSX Finder, Go->Connect to Server... and type `smb://Guest:@local.doma.in` in Server Address.
  A list with shared folders will display: mount (at least) the app share.

The app share is the important one, but you still have access to the home folder in case you want to make changes there.

Open `app` and you will see a list of folders, something like this:

```
logs
public_html
public\_html.20160114_1138
public_html.bak
public\_html.20160113_1452
```

`public\_html` and `public\_html.bak` are symbolic links to the matching `public\_html.<date>_<time>` folders. 

`public\_html` contains your Drupal codebase, to be edited with your favorite editor or sync'ed with your IDE workspace.

On OSX, you can mount the app share onto a local path, so it is easier to access by 3rd. party software, for example:
`mkdir -p /Users/me/drupsible/shares/mydrupal`
`sudo mount -t smbfs "smb://Guest:@local.doma.in/app" /Users/me/drupsible/shares/mydrupal`

## Work your Drupsible code
Under the roles share, you have access to edit all the Ansible roles that Drupsible uses. This is only intended for advanced users who want to modify the internals of Drupsible.

## Logs available!
The logs folder contains:
- access.log (Apache access log)
- drupal.log (Drupal syslog events)
- error.log (Apache error log

You can always disable syslog events in the ``ansible/playbooks/group_vars/<app_name>-local/deploy.yml`` deploy_syslog_enabled parameter (yes/no).

## Ports
On the server, this service requires ports 137, 138, 139 and 445 to be whitelisted/opened in the firewall.
