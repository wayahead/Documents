# VPNHood Setup

## Install

```
# for x64 architecture
$ sudo su -c "bash <( wget -qO- https://github.com/vpnhood/VpnHood/releases/latest/download/VpnHoodServer-linux-x64.sh)"
# for arm64
$ sudo su -c "bash <( wget -qO- https://github.com/vpnhood/VpnHood/releases/latest/download/VpnHoodServer-linux-arm64.sh)"
```

## Manage Service

```
$ sudo systemctl stop VpnHoodServer
$ sudo systemctl start VpnHoodServer
```

## Generate Access Key

```
$ sudo /opt/VpnHoodServer/vhserver gen
```

### change VpnHood ServerIP in access key

```
$ dotnet VpnHoodServer.dll gen -ep 10.11.12.13
```

## Uninstall

```
$ sudo systemctl stop VpnHoodServer || true && \
systemctl disable VpnHoodServer || true && \
systemctl stop VpnHoodUpdater || true && \
systemctl disable VpnHoodUpdater || true && \
systemctl daemon-reload && \
rm /etc/systemd/system/VpnHoodServer.service || true && \
rm /etc/systemd/system/VpnHoodUpdater.service || true && \
rm -rf /opt/VpnHoodServer
```

## Configure Server

### Linux

```
$ sudo vi /opt/VpnHoodServer/storage/appsettings.json
{
  "HttpAccessManager": null,
  "FileAccessManager": {
    "TcpEndPoints": [ "0.0.0.0:443", "[::]:443" ],
    "UdpEndPoints": [ "0.0.0.0:0", "[::]:0" ],
    "SslCertificatesPassword": null,
    "Tracking": {
      "TrackClientIp": false,
      "TrackLocalPort": false,
      "TrackDestinationIp": false,
      "TrackDestinationPort": false
    },
    "Session": {
      "Timeout": "01:00:00",
      "UdpTimeout": "00:01:00",
      "TcpTimeout": "00:15:00",
      "IcmpTimeout": "00:30:00",
      "MaxDatagramChannelCount": 8,
      "MaxUdpClientCount": null,
      "MaxIcmpClientCount": null,
      "TcpBufferSize": 8192,
      "TcpKernelSendBufferSize": null,
      "TcpKernelReceiveBufferSize": null
    },
    "NetFilter": {
      "ExcludeLocalNetwork": true,
      "PacketCaptureIncludeIpRanges": ["0.0.0.0/0", "[::]/0"],
      "PacketCaptureExcludeIpRanges": [],
      "IncludeIpRanges": ["0.0.0.0/0", "[::]/0"],
      "ExcludeIpRanges": []
    }
  },
  "IsDiagnoseMode": false
}
```

### Windows

```
> edit C:\Program Files\VpnHood\VpnHoodServer\storage\appsettings.json
{
  "HttpAccessManager": null,
  "FileAccessManager": {
    "TcpEndPoints": [ "0.0.0.0:443", "[::]:443" ],
    "UdpEndPoints": [ "0.0.0.0:0", "[::]:0" ],
    "SslCertificatesPassword": null,
    "Tracking": {
      "TrackClientIp": false,
      "TrackLocalPort": false,
      "TrackDestinationIp": false,
      "TrackDestinationPort": false
    },
    "Session": {
      "Timeout": "01:00:00",
      "UdpTimeout": "00:01:00",
      "TcpTimeout": "00:15:00",
      "IcmpTimeout": "00:30:00",
      "MaxDatagramChannelCount": 8,
      "MaxUdpClientCount": null,
      "MaxIcmpClientCount": null,
      "TcpBufferSize": 8192,
      "TcpKernelSendBufferSize": null,
      "TcpKernelReceiveBufferSize": null
    },
    "NetFilter": {
      "ExcludeLocalNetwork": true,
      "PacketCaptureIncludeIpRanges": ["0.0.0.0/0", "[::]/0"],
      "PacketCaptureExcludeIpRanges": [],
      "IncludeIpRanges": ["0.0.0.0/0", "[::]/0"],
      "ExcludeIpRanges": []
    }
  },
  "IsDiagnoseMode": false
}
```

## Run Server in Docker

### Automatic Installation

```
$ sudo su -c "bash <( wget -qO- https://github.com/vpnhood/VpnHood/releases/latest/download/VpnHoodServer.docker.sh)"
$ docker run -v "/opt/VpnHoodServer/storage:/app/storage" "vpnhood/vpnhoodserver" gen
```

### Manual Installation

```
$ docker pull vpnhood/vpnhoodserver:latest
$ docker run --network host --volume /opt/VpnHoodServer:/app/storage --name MyVpnHoodServer -d vpnhood/vpnhoodserver
```
