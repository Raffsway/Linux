No Linux, o **servidor DHCP** mais utilizado é o **ISC DHCP Server**. Ele é confiável e amplamente suportado para fornecer endereços IP dinamicamente aos clientes de uma rede.

---

### Instalação do ISC DHCP Server no Linux

1. **Debian/Ubuntu**:
   ```bash
   sudo apt update
   sudo apt install isc-dhcp-server
   ```

2. **CentOS/RHEL**:
   ```bash
   sudo yum install dhcp
   ```

3. **Arch Linux**:
   ```bash
   sudo pacman -S dhcp
   ```

---

### Configuração do Servidor DHCP

1. **Arquivo de configuração principal**:  
   - Local do arquivo: `/etc/dhcp/dhcpd.conf`

2. **Exemplo de configuração básica com IP "0.0.0.0" para variáveis abertas**:

   ```conf
   default-lease-time 600;  # Tempo padrão do lease
   max-lease-time 7200;     # Tempo máximo do lease

   # Configurações globais para a rede
   subnet 0.0.0.0 netmask 0.0.0.0 {
       range 0.0.0.0 0.0.0.0;  # Intervalo de endereços IP disponíveis
       option routers 0.0.0.0;  # Gateway padrão (ajustar conforme necessário)
       option subnet-mask 0.0.0.0;  # Máscara de sub-rede (ajustar conforme necessário)
       option domain-name-servers 0.0.0.0;  # Servidores DNS
       option domain-name "example.local";  # Nome do domínio (opcional)
   }
   ```

   - Substitua os valores `0.0.0.0` conforme as especificações de sua rede.

---

### Configuração da Interface de Rede

1. Edite o arquivo `/etc/default/isc-dhcp-server` para definir em qual interface o DHCP Server deve escutar.
   ```bash
   INTERFACESv4="eth0"  # Altere "eth0" para o nome da interface desejada
   ```

---

### Inicialização do Serviço DHCP

1. Reinicie o serviço para aplicar as alterações:
   ```bash
   sudo systemctl restart isc-dhcp-server
   ```

2. Verifique o status para garantir que tudo está funcionando:
   ```bash
   sudo systemctl status isc-dhcp-server
   ```

---

### Considerações Importantes

- **IP "0.0.0.0"**:
  - No exemplo acima, os endereços foram deixados como `0.0.0.0` para que possam ser ajustados de acordo com cada rede.
  - Certifique-se de substituir esses valores por configurações reais ao aplicar no ambiente de produção.

- **Logs do DHCP**:
  - Para depuração, você pode verificar os logs em:
    ```bash
    tail -f /var/log/syslog
    ```

- **Configurações de Firewall**:
  - Certifique-se de permitir o tráfego DHCP (porta **67/UDP** para o servidor e **68/UDP** para clientes).

Com essa configuração, o servidor DHCP está preparado para ser personalizado e adaptado conforme a topologia de rede do usuário.