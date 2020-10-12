# Ansible Role: Firewall

This role manages an iptables-based firewall on Debian/Ubuntu, RHEL/CentOS and Fedora and opensuse servers.

## Here be Dragons!

This role disables services like `firewalld` and `ufw` on RHEL and Ubuntu derivatives respectively by default. If you don't want that, check the variables.

## Known issues

None.

## Requirements

No special requirements; note that this role requires root access, so either run it in a playbook with a global `become: yes`, or invoke the role in your playbook like:

    - hosts: foobar
      roles:
        - role: ansible-role-firewall
          become: yes

## Role Variables

Available variables are listed below, along with default values (see `defaults/main.yml`):

    firewall_state: started
    firewall_enabled_at_boot: true

Start and enable the firewall at boot.

    firewall_allowed_tcp_ports: [22]

Open TCP Ports. This can be a single port a list of ports. Default is port 22 for SSH as it is needed for Ansible.

    firewall_allowed_udp_ports: []

Open UDP Ports. This can be a single port a list of ports. Default is empty.

    firewall_forwarded_tcp_ports: []
      # - src: "2222"
      #   dest: "22"
      # - src: "8080"
      #   dest: "80"

Forward TCP port `src` to `dest`.

    firewall_forwarded_udp_ports: []
      # - src: "5353"
      #   dest: "53"

Forward UDP port `src` to `dest`.

    firewall_custom_rules: []
      # - "iptables -A INPUT -p tcp --dport 8080 -s 192.168.1.1 -j ACCEPT"

Add custom IPv4 rules to the firewall. The example allows access on port 8080 from IP 192.168.1.1. Read into iptables if you need to configure something here.

    firewall_ip6_custom_rules: []
      # - "ip6tables -A INPUT -p tcp --dport 8080 -s ff02::fb -j ACCEPT"

Add custom IPv6 rules to the firewall. The example allows access on port 8080 from IP ff02::fb. Read into ip6tables if you need to configure something here.

    firewall_log_dropped_packets: true

Log dropped packets to syslog. Messages will be prefixed with `Dropped by firewall: `.

    firewall_disable_firewalld: false
    firewall_disable_ufw: false

Set to `true` to disable `firewalld` or `ufw` on RHEL and Ubuntu derivatives respectively.

## Dependencies

None.

## OS Compatibility

This role ensures that it is not used against unsupported or untested operating systems by checking, if the right distribution name and major version number are present in a dedicated variable named like `<role-name>_stable_os`. You can find the variable in the role's default variable file at `defaults/main.yml`:

    common_stable_os:
      - Debian 10
      - Ubuntu 18
      - CentOS 7
      - Fedora 30

If the combination of distribution and major version number do not match the target system, the role will fail. To allow the role to work add the distribution name and major version name to that variable and you are good to go. But please test the new combination first!

Kudos to [HarryHarcourt](https://github.com/HarryHarcourt) for this idea!

## Example Playbook

    ---
    - name: "Run role."
      hosts: all
      become: yes
      roles:
        - ansible-role-firewall

## Acknowledgements

This role is heavily based on the work by [Jeff Geerling](https://www.jeffgeerling.com/): [Ansible Role: Firewall (iptables)](https://github.com/geerlingguy/ansible-role-firewall). Kudos to him for his great work!

## Contributing

Please feel free to open issues if you find any bugs, problems or if you see room for improvement. Also feel free to contact me anytime if you want to ask or discuss something.

## Disclaimer

This role is provided AS IS and I can and will not guarantee that the role works as intended, nor can I be accountable for any damage or misconfiguration done by this role. Study the role thoroughly before using it.

## License

MIT

## Author Information

This role was created in 2014 by [Jeff Geerling](https://www.jeffgeerling.com/), author of [Ansible for DevOps](https://www.ansiblefordevops.com/).
