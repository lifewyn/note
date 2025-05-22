---
time: 2025-03-18
tags:
  - git
---

  
#### docker inspect gitlab

[

    {

        "Id": "d0631ffd76e282f108649a119f96d111c0ec48a3acaaeb0e26556494d2385679",

        "Created": "2021-11-25T08:56:33.913679765Z",

        "Path": "/assets/wrapper",

        "Args": [],

        "State": {

            "Status": "created",

            "Running": false,

            "Paused": false,

            "Restarting": false,

            "OOMKilled": false,

            "Dead": false,

            "Pid": 0,

            "ExitCode": 128,

            "Error": "driver failed programming external connectivity on endpoint gitlab (1ce4fbe6aa06982a829cd1c9db61f806affbc36208ee9c47567dd54c13db9eac): Error starting userland proxy: listen tcp4 0.0.0.0:5432: bind: address already in use",

            "StartedAt": "0001-01-01T00:00:00Z",

            "FinishedAt": "0001-01-01T00:00:00Z"

        },

        "Image": "sha256:719e7e45b1e274ef2a3cc541e4b47e8a18bc74e14068567c85df9f11a8db74ef",

        "ResolvConfPath": "/var/lib/docker/containers/d0631ffd76e282f108649a119f96d111c0ec48a3acaaeb0e26556494d2385679/resolv.conf",

        "HostnamePath": "",

        "HostsPath": "/var/lib/docker/containers/d0631ffd76e282f108649a119f96d111c0ec48a3acaaeb0e26556494d2385679/hosts",

        "LogPath": "/var/lib/docker/containers/d0631ffd76e282f108649a119f96d111c0ec48a3acaaeb0e26556494d2385679/d0631ffd76e282f108649a119f96d111c0ec48a3acaaeb0e26556494d2385679-json.log",

        "Name": "/gitlab",

        "RestartCount": 0,

        "Driver": "overlay2",

        "Platform": "linux",

        "MountLabel": "",

        "ProcessLabel": "",

        "AppArmorProfile": "",

        "ExecIDs": null,

        "HostConfig": {

            "Binds": [

                "/data/docker/gitlab/logs:/var/log/gitlab:rw",

                "/data/docker/gitlab/data:/var/opt/gitlab:rw",

                "/data/docker/gitlab/config:/etc/gitlab:rw"

            ],

            "ContainerIDFile": "",

            "LogConfig": {

                "Type": "json-file",

                "Config": {

                    "max-file": "5",

                    "max-size": "10m"

                }

            },

            "NetworkMode": "bridge",

            "PortBindings": {

                "22/tcp": [

                    {

                        "HostIp": "",

                        "HostPort": "2003"

                    }

                ],

                "5432/tcp": [

                    {

                        "HostIp": "",

                        "HostPort": "5432"

                    }

                ],

                "81/tcp": [

                    {

                        "HostIp": "",

                        "HostPort": "81"

                    }

                ]

            },

            "RestartPolicy": {

                "Name": "always",

                "MaximumRetryCount": 0

            },

            "AutoRemove": false,

            "VolumeDriver": "",

            "VolumesFrom": [],

            "CapAdd": null,

            "CapDrop": null,

            "CgroupnsMode": "host",

            "Dns": [],

            "DnsOptions": [],

            "DnsSearch": [],

            "ExtraHosts": null,

            "GroupAdd": null,

            "IpcMode": "private",

            "Cgroup": "",

            "Links": null,

            "OomScoreAdj": 0,

            "PidMode": "",

            "Privileged": false,

            "PublishAllPorts": false,

            "ReadonlyRootfs": false,

            "SecurityOpt": null,

            "UTSMode": "",

            "UsernsMode": "",

            "ShmSize": 67108864,

            "Runtime": "runc",

            "ConsoleSize": [

                0,

                0

            ],

            "Isolation": "",

            "CpuShares": 0,

            "Memory": 10737418240,

            "NanoCpus": 0,

            "CgroupParent": "",

            "BlkioWeight": 0,

            "BlkioWeightDevice": null,

            "BlkioDeviceReadBps": null,

            "BlkioDeviceWriteBps": null,

            "BlkioDeviceReadIOps": null,

            "BlkioDeviceWriteIOps": null,

            "CpuPeriod": 0,

            "CpuQuota": 0,

            "CpuRealtimePeriod": 0,

            "CpuRealtimeRuntime": 0,

            "CpusetCpus": "",

            "CpusetMems": "",

            "Devices": null,

            "DeviceCgroupRules": null,

            "DeviceRequests": null,

            "KernelMemory": 0,

            "KernelMemoryTCP": 0,

            "MemoryReservation": 0,

            "MemorySwap": 21474836480,

            "MemorySwappiness": null,

            "OomKillDisable": false,

            "PidsLimit": null,

            "Ulimits": null,

            "CpuCount": 0,

            "CpuPercent": 0,

            "IOMaximumIOps": 0,

            "IOMaximumBandwidth": 0,

            "MaskedPaths": [

                "/proc/asound",

                "/proc/acpi",

                "/proc/kcore",

                "/proc/keys",

                "/proc/latency_stats",

                "/proc/timer_list",

                "/proc/timer_stats",

                "/proc/sched_debug",

                "/proc/scsi",

                "/sys/firmware"

            ],

            "ReadonlyPaths": [

                "/proc/bus",

                "/proc/fs",

                "/proc/irq",

                "/proc/sys",

                "/proc/sysrq-trigger"

            ]

        },

        "GraphDriver": {

            "Data": {

                "LowerDir": "/var/lib/docker/overlay2/7e52bdd01bc73ca38d2d0cc8372556301bd2a8c808b66db1cb632ace6f5fc208-init/diff:/var/lib/docker/overlay2/3f2fefbe87e677d3950f55d55cc7c5baf4eb6daa31d8445ae943a10a5373022f/diff:/var/lib/docker/overlay2/d1d05599bccc0f34c815acc1fe42e88b6d477e8bcc4dbbacb5e009f84e04294a/diff:/var/lib/docker/overlay2/8b8071f52d6c4329917c367cdb184dbff36be38d4321a380f5d3e05624eae35d/diff:/var/lib/docker/overlay2/84bffacf842a2f9cb904adc2bb263f39d1ad2931ad5358abc6bbf6a12342e4e8/diff:/var/lib/docker/overlay2/26f0cdcd79cc723dd6869a6a43137f68b700e0b81b85898c678e8e11d91f07fd/diff:/var/lib/docker/overlay2/86ddd15fe83751d1c8c7915c6c315bc6b76ae80fd5ee3b7933bbd90b0dfe2a3c/diff:/var/lib/docker/overlay2/a3ff6cc62146c069f8c1ad8db696c2161248510742bfb547a520399b22c214d4/diff:/var/lib/docker/overlay2/4b41bbaba6aaaf0c337673ebb1108f8bff3ecbf28fef11c831daba1380998090/diff:/var/lib/docker/overlay2/d7ce512b814b50dafa0c36224b2b90ca0185cb3f50b203e3e7bf6ab4f9a6dbad/diff:/var/lib/docker/overlay2/e8a39d27996c9717bf028b3975e8ddfe3597bf74e2c9ef41816a6a0ee741d005/diff",

                "MergedDir": "/var/lib/docker/overlay2/7e52bdd01bc73ca38d2d0cc8372556301bd2a8c808b66db1cb632ace6f5fc208/merged",

                "UpperDir": "/var/lib/docker/overlay2/7e52bdd01bc73ca38d2d0cc8372556301bd2a8c808b66db1cb632ace6f5fc208/diff",

                "WorkDir": "/var/lib/docker/overlay2/7e52bdd01bc73ca38d2d0cc8372556301bd2a8c808b66db1cb632ace6f5fc208/work"

            },

            "Name": "overlay2"

        },

        "Mounts": [

            {

                "Type": "bind",

                "Source": "/data/docker/gitlab/config",

                "Destination": "/etc/gitlab",

                "Mode": "rw",

                "RW": true,

                "Propagation": "rprivate"

            },

            {

                "Type": "bind",

                "Source": "/data/docker/gitlab/logs",

                "Destination": "/var/log/gitlab",

                "Mode": "rw",

                "RW": true,

                "Propagation": "rprivate"

            },

            {

                "Type": "bind",

                "Source": "/data/docker/gitlab/data",

                "Destination": "/var/opt/gitlab",

                "Mode": "rw",

                "RW": true,

                "Propagation": "rprivate"

            }

        ],

        "Config": {

            "Hostname": "47.112.114.108",

            "Domainname": "",

            "User": "",

            "AttachStdin": false,

            "AttachStdout": false,

            "AttachStderr": false,

            "ExposedPorts": {

                "22/tcp": {},

                "443/tcp": {},

                "5432/tcp": {},

                "80/tcp": {},

                "81/tcp": {}

            },

            "Tty": false,

            "OpenStdin": false,

            "StdinOnce": false,

            "Env": [

                "TZ=Asia/Shanghai",

                "GITLAB_OMNIBUS_CONFIG=external_url 'http://47.112.114.108:81'\n",

                "PATH=/opt/gitlab/embedded/bin:/opt/gitlab/bin:/assets:/usr/local/sbin:/usr/local/bin:/usr/sbin:/usr/bin:/sbin:/bin",

                "LANG=C.UTF-8",

                "TERM=xterm"

            ],

            "Cmd": [

                "/assets/wrapper"

            ],

            "Healthcheck": {

                "Test": [

                    "CMD-SHELL",

                    "/opt/gitlab/bin/gitlab-healthcheck --fail --max-time 10"

                ],

                "Interval": 60000000000,

                "Timeout": 30000000000,

                "Retries": 5

            },

            "Image": "gitlab/gitlab-ce:12.8.1-ce.0",

            "Volumes": {

                "/etc/gitlab": {},

                "/var/log/gitlab": {},

                "/var/opt/gitlab": {}

            },

            "WorkingDir": "",

            "Entrypoint": null,

            "OnBuild": null,

            "Labels": {

                "com.docker.compose.config-hash": "93f0c67e7d9638a96cea6fdd07065b9ec7cb2c0deff498bc10cb8d7c61201785",

                "com.docker.compose.container-number": "1",

                "com.docker.compose.oneoff": "False",

                "com.docker.compose.project": "deploy",

                "com.docker.compose.project.config_files": "docker-compose.yml",

                "com.docker.compose.project.working_dir": "/home/docker/docker-compose/deploy",

                "com.docker.compose.service": "gitlab",

                "com.docker.compose.version": "1.27.4"

            }

        },

        "NetworkSettings": {

            "Bridge": "",

            "SandboxID": "1a91d58e4c1013e0d62a9308d8d8c410ed037399fc441ef0670fe463ec984374",

            "HairpinMode": false,

            "LinkLocalIPv6Address": "",

            "LinkLocalIPv6PrefixLen": 0,

            "Ports": {},

            "SandboxKey": "/var/run/docker/netns/1a91d58e4c10",

            "SecondaryIPAddresses": null,

            "SecondaryIPv6Addresses": null,

            "EndpointID": "1ce4fbe6aa06982a829cd1c9db61f806affbc36208ee9c47567dd54c13db9eac",

            "Gateway": "",

            "GlobalIPv6Address": "",

            "GlobalIPv6PrefixLen": 0,

            "IPAddress": "172.17.0.8",

            "IPPrefixLen": 16,

            "IPv6Gateway": "",

            "MacAddress": "02:42:ac:11:00:08",

            "Networks": {

                "bridge": {

                    "IPAMConfig": null,

                    "Links": null,

                    "Aliases": null,

                    "NetworkID": "37846c3d11f0ae697ed878c74a4da9503eecda86a282ebf46d63e059ee4bb026",

                    "EndpointID": "1ce4fbe6aa06982a829cd1c9db61f806affbc36208ee9c47567dd54c13db9eac",

                    "Gateway": "",

                    "IPAddress": "172.17.0.8",

                    "IPPrefixLen": 16,

                    "IPv6Gateway": "",

                    "GlobalIPv6Address": "",

                    "GlobalIPv6PrefixLen": 0,

                    "MacAddress": "02:42:ac:11:00:08",

                    "DriverOpts": null

                }

            }

        }

    }

]