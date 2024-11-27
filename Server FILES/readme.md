Aqui está um guia básico de como configurar um **servidor de arquivos** no Linux utilizando **Samba** (para compartilhamento de arquivos entre sistemas Linux e Windows) e **NFS** (para compartilhamento de arquivos entre sistemas Linux). Vou te guiar por ambas as opções para que você tenha uma visão geral.

### **Configuração de um Servidor de Arquivos com Samba (para compartilhamento com Windows e Linux)**

1. **Instalar o Samba**

   Primeiramente, você precisa instalar o Samba no servidor:

   - **Debian/Ubuntu**:
     ```bash
     sudo apt update
     sudo apt install samba
     ```

   - **CentOS/RHEL**:
     ```bash
     sudo yum install samba samba-client samba-common
     ```

2. **Configurar o Samba**

   O arquivo principal de configuração do Samba é o `smb.conf`, localizado em `/etc/samba/smb.conf`.

   Para editar este arquivo:

   ```bash
   sudo nano /etc/samba/smb.conf
   ```

   Aqui está um exemplo básico de configuração:

   ```ini
   [global]
   workgroup = WORKGROUP
   server string = Servidor Samba
   security = user
   map to guest = Bad User

   [compartilhamento]
   path = /srv/samba/compartilhamento
   browsable = yes
   writable = yes
   guest ok = yes
   ```

   Explicação:
   - **[compartilhamento]**: Define o nome do compartilhamento. Este será o nome da pasta compartilhada que você verá na rede.
   - **path**: Define o caminho para a pasta que será compartilhada.
   - **writable**: Permite escrita no compartilhamento.
   - **guest ok**: Permite que qualquer usuário sem autenticação acesse o compartilhamento.

3. **Criar a Pasta para Compartilhamento**

   Crie o diretório que será compartilhado:

   ```bash
   sudo mkdir -p /srv/samba/compartilhamento
   sudo chmod 0777 /srv/samba/compartilhamento
   ```

4. **Reiniciar o Samba**

   Após editar a configuração, reinicie o serviço do Samba:

   ```bash
   sudo systemctl restart smbd
   sudo systemctl enable smbd
   ```

5. **Testar o Compartilhamento**

   Para verificar se o compartilhamento está funcionando, você pode usar o comando `smbclient` para testar:

   ```bash
   smbclient -L localhost -U%
   ```

   Isso deve listar os compartilhamentos disponíveis no seu servidor Samba.

### **Configuração de um Servidor de Arquivos com NFS (para Linux a Linux)**

1. **Instalar o NFS**

   - **Debian/Ubuntu**:
     ```bash
     sudo apt update
     sudo apt install nfs-kernel-server
     ```

   - **CentOS/RHEL**:
     ```bash
     sudo yum install nfs-utils
     ```

2. **Configurar o NFS**

   O arquivo principal de configuração do NFS é o `/etc/exports`.

   Para editar o arquivo de configurações:

   ```bash
   sudo nano /etc/exports
   ```

   Exemplo de configuração:

   ```ini
   /srv/nfs/compartilhamento 0.0.0.0/0(rw,sync,no_subtree_check) # Subistitua "0.0.0.0/24" pelo endereço da sua rede
   ```

   Explicação:
   - **/srv/nfs/compartilhamento**: O diretório que será compartilhado.
   - **0.0.0.0/0**: Define a rede que pode acessar o compartilhamento. # Subistitua "0.0.0.0/24" pelo endereço da sua rede
   - **rw**: Permite leitura e escrita.
   - **sync**: Garante que as alterações no arquivo sejam sincronizadas antes de serem confirmadas.
   - **no_subtree_check**: Impede a verificação de subárvores (melhora o desempenho).

3. **Criar o Diretório para o Compartilhamento**

   Crie o diretório que será compartilhado:

   ```bash
   sudo mkdir -p /srv/nfs/compartilhamento
   sudo chmod 777 /srv/nfs/compartilhamento
   ```

4. **Reiniciar o Serviço NFS**

   Após configurar o arquivo de exportação, reinicie o serviço NFS:

   ```bash
   sudo systemctl restart nfs-kernel-server
   sudo systemctl enable nfs-kernel-server
   ```

5. **Verificar os Compartilhamentos**

   Verifique os compartilhamentos NFS com:

   ```bash
   sudo exportfs -v
   ```

6. **Montar o Compartilhamento no Cliente NFS**

   No cliente NFS, monte o compartilhamento NFS com o seguinte comando:

   ```bash
   sudo mount -t nfs 192.168.1.10:/srv/nfs/compartilhamento /mnt
   ```

   Isso montará o diretório compartilhado no cliente.

---

### **Considerações Finais**

- **Samba**: Ideal para compartilhamento entre sistemas Linux e Windows. Ele permite configurações flexíveis de permissões e autenticação.
- **NFS**: Melhor para ambientes somente Linux, oferecendo um compartilhamento de arquivos eficiente e mais rápido para servidores Linux.

Escolha o que melhor se adapta ao seu ambiente e necessidades de rede.