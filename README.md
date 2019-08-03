# Keepalive


Keepalived é um software de roteamento escrito em C.

O objetivo principal deste projeto é fornecer instalações simples e robustas para balanceamento de carga e alta disponibilidade para sistemas Linux e infra-estruturas baseadas em Linux. A estrutura do Loadbalancing depende do módulo de kernel do Servidor Virtual Linux (IPVS) conhecido e amplamente utilizado que oferece o balanceamento de carga Layer4. OKeepalived implementa um conjunto de damas para manter e gerir de forma dinâmica e adaptável pool de servidores balanceados de acordo com a sua saúde. Por outro lado, a alta disponibilidade é alcançada pelo protocolo VRRP.

O VRRP é um tijolo fundamental para o failover do roteador. Além disso, o Keepalived implementa um conjunto de ganchos na máquina de estados finitos VRRP que fornece interações de protocolo de baixo nível e alta velocidade.

Os frameworks Keepalived podem ser usados de forma independente ou todos juntos para fornecer infra-estruturas resilientes.

## Instalação

Toda a instalação é feita em **root**

### Debian

Instale o keepalive com o comando:

`apt-get install keepalived`

### CentOS

instale o keepalive com o comando:

 `yum install keeepalived`

Configuração
------------

Edite o arquivo /etc/keepalived/keepalived.conf, usando o editor de sua preferência

**Onde:** global\_defs sáo as definições globais, vrrp\_instance o nome da instância, virtual\_ipaddress o IP virtual e state se MASTER ou BACKUP

exemplo:

 ```
global_defs {
notification_email {
acassen@firewall.loc
ailover@firewall.loc
sysadmin@firewall.loc
}
notification_email_from Alexandre.Cassen@firewall.loc
smtp_server 192.168.200.1
smtp_connect_timeout 30
router_id LVS_DEVEL
}

vrrp_instance TESTE {
state MASTER
interface eth0
virtual_router_id 51
priority 100
advert_int 1
authentication {
auth_type PASS
auth_pass 1111
}
virtual_ipaddress {
192.168.1.254
} 
}
```

**Obs**: para o servidor de backup, altere o priority para um valor menor que o MASTER, caso queira que rode um script insira a seguinte linha abaixo de priority

`notify "caminho para o script"`

**Obs1**: O $3 é o status do servidor (MASTER/BACKUP)

 Inicie o serviço com o comando:

`service keepalived start`