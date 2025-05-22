
sunshine@sunshine-VirtualBox:/opt$ ssh -T -v git@192.168.30.21 -p 9022
OpenSSH_8.9p1 Ubuntu-3ubuntu0.11, OpenSSL 3.0.2 15 Mar 2022
debug1: Reading configuration data /home/sunshine/.ssh/config
debug1: /home/sunshine/.ssh/config line 5: Applying options for 192.168.30.21
debug1: Reading configuration data /etc/ssh/ssh_config
debug1: /etc/ssh/ssh_config line 19: include /etc/ssh/ssh_config.d/*.conf matched no files
debug1: /etc/ssh/ssh_config line 21: Applying options for *
debug1: Connecting to 192.168.30.21 [192.168.30.21] port 9022.
debug1: Connection established.
debug1: identity file /home/sunshine/.ssh/id_rsa type -1
debug1: identity file /home/sunshine/.ssh/id_rsa-cert type -1
debug1: identity file /home/sunshine/.ssh/id_ecdsa type -1
debug1: identity file /home/sunshine/.ssh/id_ecdsa-cert type -1
debug1: identity file /home/sunshine/.ssh/id_ecdsa_sk type -1
debug1: identity file /home/sunshine/.ssh/id_ecdsa_sk-cert type -1
debug1: identity file /home/sunshine/.ssh/id_ed25519 type 3
debug1: identity file /home/sunshine/.ssh/id_ed25519-cert type -1
debug1: identity file /home/sunshine/.ssh/id_ed25519_sk type -1
debug1: identity file /home/sunshine/.ssh/id_ed25519_sk-cert type -1
debug1: identity file /home/sunshine/.ssh/id_xmss type -1
debug1: identity file /home/sunshine/.ssh/id_xmss-cert type -1
debug1: identity file /home/sunshine/.ssh/id_dsa type -1
debug1: identity file /home/sunshine/.ssh/id_dsa-cert type -1
debug1: Local version string SSH-2.0-OpenSSH_8.9p1 Ubuntu-3ubuntu0.11
debug1: Remote protocol version 2.0, remote software version OpenSSH_8.9p1 Ubuntu-3ubuntu0.11
debug1: compat_banner: match: OpenSSH_8.9p1 Ubuntu-3ubuntu0.11 pat OpenSSH* compat 0x04000000
debug1: Authenticating to 192.168.30.21:9022 as 'git'
debug1: load_hostkeys: fopen /home/sunshine/.ssh/known_hosts2: No such file or directory
debug1: load_hostkeys: fopen /etc/ssh/ssh_known_hosts: No such file or directory
debug1: load_hostkeys: fopen /etc/ssh/ssh_known_hosts2: No such file or directory
debug1: SSH2_MSG_KEXINIT sent
debug1: SSH2_MSG_KEXINIT received
debug1: kex: algorithm: curve25519-sha256
debug1: kex: host key algorithm: ssh-ed25519
debug1: kex: server->client cipher: chacha20-poly1305@openssh.com MAC: <implicit> compression: none
debug1: kex: client->server cipher: chacha20-poly1305@openssh.com MAC: <implicit> compression: none
debug1: expecting SSH2_MSG_KEX_ECDH_REPLY
debug1: SSH2_MSG_KEX_ECDH_REPLY received
debug1: Server host key: ssh-ed25519 SHA256:jTnOFfQNvE/CJzq2d02o0pF7/HDRIVdi/wKgYz7sc+c
debug1: load_hostkeys: fopen /home/sunshine/.ssh/known_hosts2: No such file or directory
debug1: load_hostkeys: fopen /etc/ssh/ssh_known_hosts: No such file or directory
debug1: load_hostkeys: fopen /etc/ssh/ssh_known_hosts2: No such file or directory
debug1: Host '[192.168.30.21]:9022' is known and matches the ED25519 host key.
debug1: Found key in /home/sunshine/.ssh/known_hosts:39
debug1: ssh_packet_send2_wrapped: resetting send seqnr 3
debug1: rekey out after 134217728 blocks
debug1: SSH2_MSG_NEWKEYS sent
debug1: expecting SSH2_MSG_NEWKEYS
debug1: ssh_packet_read_poll2: resetting read seqnr 3
debug1: SSH2_MSG_NEWKEYS received
debug1: rekey in after 134217728 blocks
debug1: Will attempt key: /home/sunshine/.ssh/id_rsa
debug1: Will attempt key: /home/sunshine/.ssh/id_ecdsa
debug1: Will attempt key: /home/sunshine/.ssh/id_ecdsa_sk
debug1: Will attempt key: /home/sunshine/.ssh/id_ed25519 ED25519 SHA256:lO1VFqkFHQaIfx+npwyh4cRXy/D0xJhVHDJ8pjx7HC8
debug1: Will attempt key: /home/sunshine/.ssh/id_ed25519_sk
debug1: Will attempt key: /home/sunshine/.ssh/id_xmss
debug1: Will attempt key: /home/sunshine/.ssh/id_dsa
debug1: SSH2_MSG_EXT_INFO received
debug1: kex_input_ext_info: server-sig-algs=<ssh-ed25519,sk-ssh-ed25519@openssh.com,ssh-rsa,rsa-sha2-256,rsa-sha2-512,ssh-dss,ecdsa-sha2-nistp256,ecdsa-sha2-nistp384,ecdsa-sha2-nistp521,sk-ecdsa-sha2-nistp256@openssh.com,webauthn-sk-ecdsa-sha2-nistp256@openssh.com>
debug1: kex_input_ext_info: publickey-hostbound@openssh.com=<0>
debug1: SSH2_MSG_SERVICE_ACCEPT received
debug1: Authentications that can continue: publickey
debug1: Next authentication method: publickey
debug1: Trying private key: /home/sunshine/.ssh/id_rsa
debug1: Trying private key: /home/sunshine/.ssh/id_ecdsa
debug1: Trying private key: /home/sunshine/.ssh/id_ecdsa_sk
debug1: Offering public key: /home/sunshine/.ssh/id_ed25519 ED25519 SHA256:lO1VFqkFHQaIfx+npwyh4cRXy/D0xJhVHDJ8pjx7HC8
debug1: Server accepts key: /home/sunshine/.ssh/id_ed25519 ED25519 SHA256:lO1VFqkFHQaIfx+npwyh4cRXy/D0xJhVHDJ8pjx7HC8
Authenticated to 192.168.30.21 ([192.168.30.21]:9022) using "publickey".
debug1: channel 0: new [client-session]
debug1: Requesting no-more-sessions@openssh.com
debug1: Entering interactive session.
debug1: pledge: filesystem
debug1: client_input_global_request: rtype hostkeys-00@openssh.com want_reply 0
debug1: client_input_hostkeys: searching /home/sunshine/.ssh/known_hosts for [192.168.30.21]:9022 / (none)
debug1: client_input_hostkeys: searching /home/sunshine/.ssh/known_hosts2 for [192.168.30.21]:9022 / (none)
debug1: client_input_hostkeys: hostkeys file /home/sunshine/.ssh/known_hosts2 does not exist
debug1: client_input_hostkeys: no new or deprecated keys from server
debug1: Remote: /var/opt/gitlab/.ssh/authorized_keys:6: key options: command user-rc
debug1: Remote: /var/opt/gitlab/.ssh/authorized_keys:6: key options: command user-rc
debug1: Sending environment.
debug1: channel 0: setting env LC_ADDRESS = "zh_CN.UTF-8"
debug1: channel 0: setting env LC_NAME = "zh_CN.UTF-8"
debug1: channel 0: setting env LC_MONETARY = "zh_CN.UTF-8"
debug1: channel 0: setting env LC_PAPER = "zh_CN.UTF-8"
debug1: channel 0: setting env LANG = "en_US.UTF-8"
debug1: channel 0: setting env LC_IDENTIFICATION = "zh_CN.UTF-8"
debug1: channel 0: setting env LC_TELEPHONE = "zh_CN.UTF-8"
debug1: channel 0: setting env LC_MEASUREMENT = "zh_CN.UTF-8"
debug1: channel 0: setting env LC_TIME = "zh_CN.UTF-8"
debug1: channel 0: setting env LC_NUMERIC = "zh_CN.UTF-8"
Welcome to GitLab, @root!
debug1: client_input_channel_req: channel 0 rtype exit-status reply 0
debug1: client_input_channel_req: channel 0 rtype eow@openssh.com reply 0
debug1: channel 0: free: client-session, nchannels 1
Transferred: sent 2804, received 2792 bytes, in 0.1 seconds
Bytes per second: sent 24210.3, received 24106.7
debug1: Exit status 0
