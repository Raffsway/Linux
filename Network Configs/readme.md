Aqui está uma versão organizada e otimizada das configurações para o **Netplan** e o método tradicional usando o arquivo `/etc/network/interfaces`:

---

## **Configuração de Rede no Ubuntu: Usando Netplan e /etc/network/interfaces**

O **Netplan** é a ferramenta padrão para configurações de rede em distribuições Linux baseadas no Ubuntu (a partir da versão 17.10), substituindo o antigo sistema `/etc/network/interfaces`. O Netplan usa arquivos YAML para configurar a rede e oferece flexibilidade para trabalhar com diferentes backends de rede, como `networkd` e `NetworkManager`.

---

### **Usando o Netplan para Configuração de Rede**

#### **Passo 1: Verificar o nome das interfaces de rede**

Primeiro, identifique as interfaces de rede disponíveis no seu sistema com o comando:

```bash
ip a
```

Isso exibirá a lista de interfaces de rede, como `enp3s0`, `eth0`, `ens33`, etc.

#### **Passo 2: Localizar os arquivos de configuração do Netplan**

Os arquivos de configuração do Netplan estão localizados em `/etc/netplan/`. Para ver os arquivos disponíveis, execute:

```bash
ls /etc/netplan/
```

Exemplo de arquivos: `00-installer-config.yaml`, `01-netcfg.yaml`.

#### **Passo 3: Editar o arquivo de configuração do Netplan**

Edite o arquivo de configuração escolhido. Por exemplo, para editar o arquivo `01-netcfg.yaml`:

```bash
sudo nano /etc/netplan/01-netcfg.yaml
```

Agora, você pode configurar a interface de rede para IP estático ou DHCP.

#### **Configuração de IP Estático no Netplan**

Aqui está um exemplo de configuração para um IP estático:

```yaml
network:
  version: 2
  renderer: networkd
  ethernets:
    enp3s0:  # Substitua pelo nome da sua interface
      dhcp4: no
      addresses:
        - 192.168.1.10/24  # Substitua pelo IP desejado e a máscara
      gateway4: 192.168.1.1  # Substitua pelo IP do gateway
      nameservers:
        addresses:
          - 8.8.8.8  # Servidor DNS primário
          - 8.8.4.4  # Servidor DNS secundário
```

#### **Configuração de DHCP (IP Dinâmico) no Netplan**

Se você preferir usar DHCP, a configuração seria:

```yaml
network:
  version: 2
  renderer: networkd
  ethernets:
    enp3s0:  # Substitua pelo nome da sua interface
      dhcp4: yes
```

#### **Passo 4: Aplicar as Configurações**

Depois de editar o arquivo YAML, aplique as configurações com:

```bash
sudo netplan apply
```

Isso fará com que o Netplan aplique imediatamente as configurações de rede.

#### **Passo 5: Verificar a Configuração de Rede**

Para verificar se as configurações foram aplicadas corretamente, execute:

```bash
ip a
ping 8.8.8.8  # Teste a conectividade com a internet
```

---

### **Configuração de Rede Usando /etc/network/interfaces (Método Tradicional)**

Se o seu sistema não usar o **Netplan**, você pode configurar a rede diretamente no arquivo **/etc/network/interfaces**.

#### **Passo 1: Editar o Arquivo de Configuração**

Para editar o arquivo `/etc/network/interfaces`, use o `nano`:

```bash
sudo nano /etc/network/interfaces
```

#### **Configuração de IP Estático (Exemplo)**

Aqui está um exemplo de configuração de uma interface Ethernet com IP estático (supondo que a interface seja `eth0`):

```bash
# Configuração da interface loopback
auto lo
iface lo inet loopback

# Configuração de interface Ethernet com IP estático
auto eth0
iface eth0 inet static
    address 192.168.1.10
    netmask 255.255.255.0
    gateway 192.168.1.1
    dns-nameservers 8.8.8.8 8.8.4.4
```

#### **Configuração de DHCP (IP Dinâmico)**

Se você quiser que a interface obtenha o IP automaticamente via DHCP:

```bash
# Configuração da interface loopback
auto lo
iface lo inet loopback

# Configuração de interface Ethernet para DHCP
auto eth0
iface eth0 inet dhcp
```

#### **Configuração de Várias Interfaces**

Se houver mais de uma interface de rede, como `eth1`, você pode adicionar uma configuração adicional:

```bash
# Configuração de uma segunda interface com IP estático
auto eth1
iface eth1 inet static
    address 192.168.2.10
    netmask 255.255.255.0
    gateway 192.168.2.1
    dns-nameservers 8.8.8.8 8.8.4.4
```

#### **Passo 2: Reiniciar os Serviços de Rede**

Depois de salvar as alterações no arquivo `/etc/network/interfaces`, reinicie os serviços de rede para aplicar as configurações:

```bash
sudo systemctl restart networking
```

Ou, caso o seu sistema não utilize `systemd`:

```bash
sudo /etc/init.d/networking restart
```

#### **Passo 3: Verificar a Conexão de Rede**

Depois de reiniciar a rede, verifique se o IP foi configurado corretamente com:

```bash
ip a
ping 8.8.8.8  # Teste a conectividade
```

---

### **Exemplo Completo de Configuração (Método Tradicional)**

Aqui está um ``exemplo`` completo de configuração de duas interfaces, uma com IP estático e outra usando DHCP:

```bash
# Interfaces de rede

# Configuração da interface loopback
auto lo
iface lo inet loopback

# Configuração da interface eth0 (IP Estático)
auto eth0
iface eth0 inet static
    address 192.168.1.10
    netmask 255.255.255.0
    gateway 192.168.1.1
    dns-nameservers 8.8.8.8 8.8.4.4

# Configuração da interface eth1 (DHCP)
auto eth1
iface eth1 inet dhcp
```

---

### **Conclusão**

- **Netplan** é a ferramenta moderna para configurar a rede em distribuições Linux baseadas no Ubuntu, utilizando arquivos YAML.
- O método tradicional de configurar redes, **/etc/network/interfaces**, ainda é utilizado em algumas versões ou distribuições.
- Após configurar, sempre reinicie os serviços de rede e verifique as configurações com `ip a` ou `ping` para testar a conectividade.

Se precisar de mais ajuda ou ajustes nas configurações, fique à vontade para perguntar!