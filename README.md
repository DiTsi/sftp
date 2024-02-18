# sftp
Simple Ansible role to configure OpenSSH SFTP server and create users with dirs. Authentication using ssh key.

After using you will get directory structure like:
```
/sftp
 └─ user
    └─ home
```
or more complex:
```
/sftp
 ├─ user1
 │  ├─ home
 │  └─ media
 └─ user2
    ├─ home
    └─ docs
```
where
- `/sftp` - root SFTP directory (configure with `sftp.root_dir`)
- `user1`, `user2` - user dirs (configure with `sftp.users[0].name`)
- `home` - users home dirs (configure name with `sftp.home_name`)
- `media`, `docs` - users home dirs (configure with `sftp.users[0].dirs`)

## Features
- create home and additional user dirs
- set ssh key for authentication
- set custom permissions (mode) for directories

## Limitations
- password authentication is not supported
- no check for existing usernames on server. Check it yourself! Script will rewrite they if exist

## Using
1. Create folder roles: `mkdir roles`
2. Clone repo to `roles/`: `git clone git@github.com:DiTsi/sftp.git roles/sftp`
3. Create file `hosts` with your host:
    ```ini
    [all]
    host ansible_host=123.123.123.123 ansible_user=user
    ```
4. Create playbook `playbook.yml` and set variables:
    ```yaml
    - hosts: host
      roles:
      - role: sftp
        vars:
          sftp:
            group: sftpstorage
            home_name: home
            root_dir: /sftp
            shell: /sbin/nologin
            users:
              - name: user1
                ssh_key: |
                  ssh-rsa AAAAB3NzaC1yc2EAAAADAQABAAABgQDF9WjMaDHZ7Xz5224Zdvl+z1RA5zp09vkJ1IGcmBMv/0DANm4CyW2Cao2A1qdaqYPGqsGZeD+SNFMHilyvUbMS7uJJrLmZNp93N+gJPKLgvTw/+3z4zZ3frdSXCSNGKPF6yPifceUi9GjBroNv4EUAaY4nlVL44+K+TotvdwUuTn33kh1rXuksqgcoBcM8QdMkCLe5ErQPBxz1GrnI6oZPKCk0gqu84+uswWPE7bMyQyv/axroKoAaRglgjEeZTfS4pfpI5LCDPE4hErEKL9s4QXxTIiTDQaLjOIMHda+pGFVbvxGVjbZaiqJb3lctTWND+HLhgNe6jLuLJ7eznx3WNo3Xm0vKImNVnKrsiOVZspnykuR6EYKJPUi2UWkg05D2nDUmfkGIMk5DGIt83nYjyMuXT+F0+ZgK230PqlLgyD7SbmRZSH7c6GPuKWfgBHSro+5Uxy4hJS8oxmt3Li9fH/5JpOm1mZZiYmCeW5jvA75cuni3q846/VzvM8Hs0JM=
                dirs:
                  - name: media
              - name: user2
                ssh_key: |
                  ssh-rsa AAAAB3NzaC1yc2EAAAADAQABAAABgQDF9WjMaDHZ7Xz5224Zdvl+z1RA5zp09vkJ1IGcmBMv/0DANm4CyW2Cao2A1qdaqYPGqsGZeD+SNFMHilyvUbMS7uJJrLmZNp93N+gJPKLgvTw/+3z4zZ3frdSXCSNGKPF6yPifceUi9GjBroNv4EUAaY4nlVL44+K+TotvdwUuTn33kh1rXuksqgcoBcM8QdMkCLe5ErQPBxz1GrnI6oZPKCk0gqu84+uswWPE7bMyQyv/axroKoAaRglgjEeZTfS4pfpI5LCDPE4hErEKL9s4QXxTIiTDQaLjOIMHda+pGFVbvxGVjbZaiqJb3lctTWND+HLhgNe6jLuLJ7eznx3WNo3Xm0vKImNVnKrsiOVZspnykuR6EYKJPUi2UWkg05D2nDUmfkGIMk5DGIt83nYjyMuXT+F0+ZgK230PqlLgyD7SbmRZSH7c6GPuKWfgBHSro+5Uxy4hJS8oxmt3Li9fH/5JpOm1mZZiYmCeW5jvA75cuni3q846/VzvM8Hs0JM=
                dirs:
                  - name: docs
                    mode: u=rwx,g=rx,o=
    ```
5. Run with `ansible-playbook -i hosts playbook.yml`

## Configuration

```yaml

```

## Connect to server
### Windows

Install [sshfs-win](https://github.com/winfsp/sshfs-win) first

You can mount network share as Network Drive using graphical interface or using PowerShell with these addresses:
  - to connect to user's home dirctory use address
    ```batch
    \\sshfs.k\<username>@<server's_ip_address>
    ```
  - to connect to user's `media` directory use:
    ```batch
    \\sshfs.k\<username>@<server's_ip_address>\..\media
    ```
For details see sshfs-win [documentation](https://github.com/winfsp/sshfs-win)
