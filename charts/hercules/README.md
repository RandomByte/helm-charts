# [Hercules](https://github.com/RandomByte/helm-charts/blob/master/charts/hercules) - Opinionated Collection of Tools to Produce and Store Various Metrics in My Home Environment

**Many Docker images used in this project are built exclusively for the armhf architecture (Raspberry Pi et al.).**

## Prerequisites
1. Have a running Kubernetes cluster on one or more Raspberry Pis
    - *The Docker images used in this project currently only support the Pis armhf architecture*
    - I highly recommend checking [this repository](https://github.com/alexellis/k8s-on-raspbian) for information on how setting up Kubernetes on Raspbian.
    - Personally I use [k3s](https://k3s.io/). I added some notes to [misc/k3s-setup.md](./misc/k3s-setup.md)
1. Have [kubectl](https://kubernetes.io/docs/tasks/tools/install-kubectl/) and [Helm](https://helm.sh/) (v3) installed on your development system

## Usage
```sh
helm repo add randombyte https://randombyte.github.io/helm-charts/
helm repo update

# TODO: Customize values

helm install hercules randombyte/hercules
```

## Development
1. Clone this repository and navigate into it
    ``` sh
    git clone https://github.com/RandomByte/helm-charts.git
    cd helm-charts/charts/hercules
    ```
1. Copy the file `values.yaml` to `prod-values.yaml`:  
    ``` sh
    cp values.yaml prod-values.yaml
    ```

1. In the file `prod-values.yaml`, replace all variables with the appropriate values
    - `<external ip>`: Enter the external IP address of one of your nodes (preferably the 'master'), that should be used by external services to communicate with the MQTT and other servers within your cluster

1. Create a helm release named "hercules" and deploy it via kubectl
    ``` sh
    helm install -f prod-values.yaml hercules .
    ```

## Upgrade
To apply changes to the running cluster execute:
```sh
helm upgrade -f prod-values.yaml hercules .
```

## Uninstall
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

## License
Released under the [MIT License](https://opensource.org/licenses/MIT).
