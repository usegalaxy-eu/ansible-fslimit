# Ansible Role: FSLimit

Limits maximum file sizes for a systemd unit

Variables:

- `ulimit_fsize_unit`: the systemd unit to limit (default: `condor.service`)

- `ulimit_fsize_soft`: the soft limit in *bytes* (default value amounts to 250 GiB)

- `ulimit_fsize_hard`: the hard limit in *bytes* (default value amounts to 1 TiB)

This role uses the "ulimit" interface (the actual syscall is `setrlimit(2)`)
which imposes limits *per process* that are inherited by child processes.

When exceeding the soft limit, a process will receive `SIGXFSZ`
(terminating the process by default, but the signal can be caught).

An unprivileged process may raise its soft limit up to the hard limit,
raising the hard limit is a privileged operation.


### Sample requirements.yml

    ---
    roles:
      - name: usegalaxy-eu.fslimit
        src: https://github.com/usegalaxy-eu/ansible-fslimit
        version: main

### Example Playbook

    ---
    - name: Set file size limits for condor service unit
      hosts: all
      roles:
        - role: usegalaxy-eu.fslimit
          become: yes
          vars:
            ulimit_fsize_unit: "condor.service"
            ulimit_fsize_soft: 268435456000
            ulimit_fsize_hard: 1073741824000
