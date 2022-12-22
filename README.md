# Huawei SUN2000 KTL L1 Inverter Modbus TCP to MQTT
Monitoring the Huawei SUN2000L KTL L1 inverter via Modbus TCP and publishing the values to MQTT

**What is this?**

This script allows to control a Huawei SUN2000L KTL L1 inverter via Modbus TCP and to publish the values in MQTT (for use in node-red, Home Assistant, OpenHab...).

I created this script because the examples I found on the internet did not work with the dongle (SDongle A52) using LAN connection and not via WiFi. It seems that the SUN2000L is quite peculiar with its Modbus TCP protocol...

I use the HuaweiSolar library (https://gitlab.com/Emilv2/huawei-solar) from Emilv2. I only extract the data I am interested in, but of course, you can include all the data provided by the inverter via Modbus TCP.

Finally, there is also the possibility to build a docker image and launch it in any environment.

A lot of information, code and knowledge extracted from:

- https://domotuto.com/conexion-del-inversor-fotovoltaico-huawei-sun2000-a-broker-mqtt-node-red/
- https://www.dropbox.com/s/9zaa1zexnr6cv60/detalles_modbus-tcp.py?dl=0
- https://community.home-assistant.io/t/integration-solar-inverter-huawei-2000l/132350/1

*Howto generate docker container*

First, download the code from this repo (you can either clone the repo or download the gzip file and extract it).
You should download it in the same computer you are going to run the container.

Modify the `huaweisolar.py` script and add any additional parameters you wish to log into the `vars` and `vars_inmediate` variables,
see the library source for the possible values of the sources, see: https://gitlab.com/Emilv2/huawei-solar/-/blob/master/src/huawei_solar/register_names.py

Then, you need to generate the docker image. Enter inside the code folder, and execute the following command:

```
docker build -t huawei-solar .
```

Once the docker image has been generated, you can use the following docker-compose service to initialize it:

```
 huawei-solar:
    container_name: huawei-solar
    restart: unless-stopped
    image: huawei-solar:latest
    environment:
      - INVERTER_IP=XXX.XXX.XXX.XXX
      - MQTT_HOST=XXX.XXX.XXX.XXX
```

Save as: `docker-compose.yml`
Run:

```
docker compose up -d
```


*Docker container*

![docker](https://raw.githubusercontent.com/ccorderor/huawei-sun2000-modbus-mqtt/main/images/huawei_docker.png)


*Node-red mqtt listeners*

![node-red](https://raw.githubusercontent.com/ccorderor/huawei-sun2000-modbus-mqtt/main/images/huawei_nodered.png)
