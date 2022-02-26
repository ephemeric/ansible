# Rsync.
rsync -cvae ssh --chmod=D0755,F0644 --exclude="*.j2" --delete-after --progress --out-format="[%t]:%o:%f:Last Modified %M" src dst:tmp/

find /etc/ -type f -mtime -3 -print0 | tar -cvzpf "$(hostname)".tgz --exclude="/etc/oath/users" --exclude="/etc/ansible/ssh-ca/ca" --exclude="/etc/passwd" --null -T -

# Server: /etc/ssh/sshd_config.
Match User c2
  AuthenticationMethods   publickey
  HostbasedAuthentication no
  RhostsRSAAuthentication no
  GSSAPIAuthentication    no
  PasswordAuthentication  no
  AllowAgentForwarding    no
  GatewayPorts            no
  X11Forwarding           no
  AllowTcpForwarding      yes
  Banner                  none
  ForceCommand            /sbin/nologin
  PermitOpen              172.16.7.131:2224

# Client: crontab.
MAILTO=""
* * * * * (ssh -p 443 -fNR 127.0.0.1:4450:127.0.0.1:22 c2@197.15.100.245 &>/dev/null &)  

# Client: /etc/ssh/sshd_config.
ClientAliveInterval 20
ClientAliveCountMax 3
AllowUsers c2@127.0.0.1

# /etc/sudoers.d/wheel (0400)
%wheel ALL=(ALL) NOPASSWD:ALL
