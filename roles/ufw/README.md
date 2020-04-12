# ufw

Configures the firewall using [ufw](https://launchpad.net/ufw).

## Usage

```yml
- role: ufw
  ufw__logging: low # must be one of: full, high, low, medium, off, on; default: low
  ufw__default_incoming_policy: allow # must be one of: allow, deny, reject; default: allow
  ufw__default_outgoing_policy: allow # must be one of: allow, deny, reject; default: allow
  ufw__rules: # default: []
    - rule: allow       # must be one of: allow, deny, limit, reject; required
      interface: 'eth0' # optional
      proto: any        # must be one of: ah, any, esp, ipv6, tcp, udp; default: any
      direction: in     # must be one of: in, incoming, out, outgoing, routed; default: in
      from_ip: any      # default: any
      to_ip: any        # default: any
      from_port: any    # optional
      to_port: 22       # optional
      log: false        # must be boolean; default: false
      comment: 'Allow access to SSH' # optional
    - ... # other rule
```

For more details, check the following resources:

* [Documentation to ufw module on Ansible.com](https://docs.ansible.com/ansible/latest/modules/ufw_module.html),
* [Manual to ufw](https://manpages.debian.org/buster/ufw/ufw.8.en.html),
* [Guide to ufw on Ubuntu.com](https://help.ubuntu.com/community/UFW).

### Removing previously added rules

:warning: Running this role by default does not delete previously added rules.

Therefore, if you want to delete any firewall rules on a target machine, you have two options:

* you can do it manually on your target machine using `ufw` command

* you can run your playbook with the following tags: `--tags all,ufw__reset`:

    ```shell
    you@control_machine:~$ ansible-playbook ... --tags all,ufw__reset
    ```

    This option disables and resets firewall to installation defaults (it uses `ufw reset` under the hood) and then
    configures the rules from scratch. It means that you will be deprived of a firewall protection for a moment
    during that operation. This may also disrupt existing ssh connections.

#### Removing firewall rules completely

To disable and reset firewall to installation defaults, you need to manually run:

```shell
admin@target_machine:~$ sudo ufw reset
```

## Debugging

* Display the status of the firewall and the list of configured rules on a target machine:

    ```shell
    admin@target_machine:~$ sudo ufw status verbose
    Status: active
    Logging: off
    Default: deny (incoming), allow (outgoing), disabled (routed)
    New profiles: skip

    To                         Action      From
    --                         ------      ----
    22/tcp                     ALLOW IN    Anywhere
    22/tcp (v6)                ALLOW IN    Anywhere (v6)
    ```

* Read logs:

    ```shell
    admin@target_machine:~$ sudo less /var/log/ufw.log
    ```
