# Getting started with FWE

-> https://github.com/aws/aws-iot-fleetwise-edge/blob/main/docs/dev-guide/edge-agent-dev-guide.md#getting-started-guide
-> Use the cfn template to spin up the ec2 graviton instance.
-> Upgrade from Ubuntu 20.04 to 22.04 (For getting compatible python version)
-> To use the ic flutter app, install ubuntu GUI
-> Do aws configure
-> Follow AWS IoT Fleetwise documentation to provision fleetwise resources and iot core resources (refer some custom cli commands below) (https://github.com/aws/aws-iot-fleetwise-edge/blob/main/docs/dev-guide/edge-agent-dev-guide.md#getting-started-on-a-development-machine)
-> The campaign won't be successfully implemented via CLI command, so once the demo.sh finishes, create a campaign on your own via the console. The signals need to be added are DI_vehicleSpeed, Vehicle.Speed, Vehicle.ID257DIspeed.DI_vehicleSpeed.

Files I have changed

1. DBC file -> speed_dbc.dbc
2. modified obd-nodes.json
3. modified obd-decoders.json
4. modified network-interfaces.json
5. Created campaign-obd-heartbeat-speed.json

->
sudo mkdir -p /etc/aws-iot-fleetwise \
&& sudo ./tools/provision.sh \
   --region eu-central-1 \
   --vehicle-name fwdemo-ec2 \
   --certificate-pem-outfile /etc/aws-iot-fleetwise/certificate.pem \
   --private-key-outfile /etc/aws-iot-fleetwise/private-key.key \
   --endpoint-url-outfile /etc/aws-iot-fleetwise/endpoint.txt \
   --vehicle-name-outfile /etc/aws-iot-fleetwise/vehicle-name.txt \
&& sudo ./tools/configure-fwe.sh \
   --input-config-file configuration/static-config.json \
   --output-config-file /etc/aws-iot-fleetwise/config-0.json \
   --log-color Yes \
   --vehicle-name `cat /etc/aws-iot-fleetwise/vehicle-name.txt` \
   --endpoint-url `cat /etc/aws-iot-fleetwise/endpoint.txt` \
   --can-bus0 vcan0 \
&& sudo ./tools/install-fwe.sh

-> 
./demo.sh --vehicle-name fwdemo-ec2 --dbc-file /home/developer/workspace/kuksa-can-provider/speed_dbc.dbc --campaign-file campaign-obd-heartbeat-speed.json --region eu-central-1

->
cd ~/workspace/aws-iot-fleetwise-edge/tools/cloud && ./clean-up.sh && ../provision.sh    --vehicle-name fwdemo-ec2    --region eu-central-1    --only-clean-up


-> To see the speed populate the IC flutter app, use kuksa-can-provider steps (https://github.com/eclipse-kuksa/kuksa-can-provider/blob/main/doc/configuration.md)
-> Once fleetwise is up and running, push the CAN messages from candump.log to see Speed messages populate the DB
