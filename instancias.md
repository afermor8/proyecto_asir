# Creación de instancias

Los servidores se crearán usando instancias de Openstack del IES Gonzalo Nazareno. Para ello me conecto a la VPN del instituto, activo el cliente openstack y accedo con mi contraseña.

```bash
sudo systemctl start openvpn

source /home/arantxa/venv/openstackclient/bin/activate

source /home/arantxa/Descargas/Proyecto-arantxa.fernandez-openrc.sh
```

Las dos instancias que voy a crear se van a configurar con cloud-init. Para ello he creado dos ficheros de configuración que se pueden encontrar en este repositorio: cloud-config-gitlab.yaml y cloud-config-harbor.yaml. 

A continuación, se procede a crear dos volúmenes para cada instancia.

```bash
openstack volume create --size 50 --image "Debian 11 Bullseye"  --bootable vol-gitlab

openstack volume create --size 50 --image "Debian 11 Bullseye"  --bootable vol-harbor
 
openstack volume create --size 10 --image "Debian 11 Bullseye"  --bootable vol-deployserver
```

Creo las instancias de la siguiente manera:

```bash
openstack server create --volume vol-gitlab \
 --flavor m1.xlarge \
 --security-group default \
 --key-name Mi_Clave \
 --network "red de arantxa.fernandez" \
 --user-data cloud-config-gitlab.yaml \
 gitlab
```

```bash
openstack server create --volume vol-harbor \
 --flavor m1.large \
 --security-group default \
 --key-name Mi_Clave \
 --network "red de arantxa.fernandez" \
 --user-data cloud-config-harbor.yaml \
 harbor
```

```bash
openstack server create --volume vol-deployserver \
 --flavor m1.normal \
 --security-group default \
 --key-name Mi_Clave \
 --network "red de arantxa.fernandez" \
 --user-data cloud-config-deploy-server.yaml \
 deployserver
```

Asigno una IP flotante a los puertos de cada instancia conectados a la red interna.

```bash
openstack floating ip create ext-net

openstack server add floating ip gitlab 172.22.200.23

openstack server add floating ip harbor 172.22.201.183

openstack server add floating ip deployserver 172.22.201.134
```

Ya podemos acceder a las máquinas:

```bash
ssh arantxa@172.22.200.23

ssh arantxa@172.22.201.183

ssh arantxa@172.22.201.134
```
