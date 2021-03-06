# Hercules

> An Opinionated Collection of Tools to Produce and Store Various Metrics in a Home Environment

**⚠️ Some Docker images used in this project are built exclusively for the armhf architecture (Raspberry Pi et al.)**

## Prerequisites
1. Have a running Kubernetes cluster on one or more Raspberry Pis
    - The Docker images used in this project currently only support the Pis armhf architecture
    - I highly recommend checking [this repository](https://github.com/alexellis/k8s-on-raspbian) for information on how setting up Kubernetes on Raspbian.
    - Personally I use [k3s](https://k3s.io/). I added some notes to [misc/k3s-setup.md](../../misc/k3s-setup/)
1. Have [kubectl](https://kubernetes.io/docs/tasks/tools/install-kubectl/) and [Helm](https://helm.sh/) (v3) installed on your development system

## Usage
### Install
#### Add Repository
```sh
helm repo add randombyte https://randombyte.github.io/helm-charts/
helm repo update
```

#### Download Default `values.yaml`
```sh
curl https://raw.githubusercontent.com/RandomByte/helm-charts/main/charts/hercules/values.yaml > config.yaml
```

In the new file `config.yaml`, replace all variables with the appropriate values:  
- `<external ip>`: Use the external IP address of one of your nodes, which should be used by external-facing services like the MQTT server

#### Create a Release in Your Cluster
```sh
helm install hercules randombyte/hercules -f config.yaml 
```

### Upgrade
To apply changes to the running cluster execute:
```sh
helm repo update
helm upgrade hercules randombyte/hercules -f config.yaml --cleanup-on-fail
```

### Uninstall
As easy as:

```sh
helm uninstall hercules
```

## Modules
Modules that are either running within the Kubernetes cluster or on external devices such as sensors. Communication happens via MQTT.

### Cluster Services
- [RandomByte/mqtt-traffic](https://github.com/RandomByte/mqtt-traffic)
- [RandomByte/mqtt-online-check](https://github.com/RandomByte/mqtt-online-check)
- [oliverlorenz/http2mqtt](https://github.com/oliverlorenz/http2mqtt)
    - To publish mqtt messages via the iOS Shortcuts app

### External Services
- [RandomByte/esp8266-mqtt-light-sensor](https://github.com/RandomByte/esp8266-mqtt-light-sensor)
- [RandomByte/mqtt-nodemotion](https://github.com/RandomByte/mqtt-nodemotion)
- [RandomByte/mqtt-wifi-scanner](https://github.com/RandomByte/mqtt-wifi-scanner)

## Development
1. Clone this repository and navigate into it
    ``` sh
    git clone https://github.com/RandomByte/helm-charts.git
    cd helm-charts/charts/hercules
    ```
1. Copy the file `values.yaml` to `config.yaml`:  
    ``` sh
    cp values.yaml config.yaml
    ```
1. In the file `config.yaml`, replace all variables with the appropriate values
1. Create a helm release named "hercules" (or upgrade an existing one) and deploy it via kubectl
    ``` sh
    helm upgrade hercules . -f config.yaml --install # --atomic
    ```

## License
Released under the [MIT License](https://opensource.org/licenses/MIT).
