Sim, o **SFTP (SSH File Transfer Protocol)** é uma alternativa mais segura ao TFTP. Ao contrário do TFTP, o SFTP utiliza o protocolo SSH (Secure Shell) para transferir arquivos de forma segura, criptografando os dados durante a transferência.

Aqui está como configurar e testar o **SFTP** no Linux:

---

### **Configuração e Testes do SFTP no Linux**

---

### **Passo 1: Instalar o OpenSSH (Se necessário)**

O SFTP faz parte do pacote OpenSSH, que geralmente já está instalado em distribuições Linux modernas. Caso não tenha o OpenSSH instalado, você pode instalar da seguinte forma:

#### 1. **Debian/Ubuntu**:
```bash
sudo apt update
sudo apt install openssh-server
```

#### 2. **CentOS/RHEL**:
```bash
sudo yum install openssh-server
```

#### 3. **Arch Linux**:
```bash
sudo pacman -S openssh
```

---

### **Passo 2: Iniciar o Serviço SSH**

Após instalar o OpenSSH, é necessário iniciar o serviço SSH para permitir conexões de SFTP:

```bash
sudo systemctl start ssh
sudo systemctl enable ssh
```

Isso garante que o serviço SSH será iniciado automaticamente em cada reinicialização do sistema.

---

### **Passo 3: Configurar o Acesso ao SFTP (Opcional)**

Se você deseja restringir o acesso ao SFTP, você pode configurar o **sshd_config** para permitir ou negar acesso a usuários específicos. Para isso, edite o arquivo de configuração do SSH:

```bash
sudo nano /etc/ssh/sshd_config
```

Adicione ou modifique as seguintes linhas para configurar as permissões de SFTP:

```conf
# Permitir apenas acesso SFTP
Subsystem sftp /usr/lib/openssh/sftp-server

# Restringir usuário para somente SFTP
Match User username
    ChrootDirectory /home/username
    ForceCommand internal-sftp
    AllowTcpForwarding no
```

Substitua `username` pelo nome do usuário que deve ter acesso restrito ao SFTP. O diretório chroot `/home/username` significa que o usuário será restringido a esse diretório.

Após editar o arquivo, reinicie o serviço SSH:

```bash
sudo systemctl restart ssh
```

---

### **Passo 4: Testar o Acesso SFTP**

#### 1. **Conectar ao Servidor via SFTP**

Para conectar ao servidor via SFTP, use o comando `sftp` do lado do cliente. No cliente, execute o seguinte comando para conectar ao servidor:

```bash
sftp username@hostname_or_ip
```

Substitua `username` pelo nome do usuário no servidor e `hostname_or_ip` pelo endereço IP ou nome do host do servidor.

#### 2. **Transferir Arquivos via SFTP**

Após conectar, você pode usar os comandos SFTP para navegar e transferir arquivos. Alguns comandos úteis do SFTP são:

- **Listar arquivos no diretório remoto**:
  ```bash
  ls
  ```

- **Mudar para um diretório remoto**:
  ```bash
  cd /path/to/directory
  ```

- **Mudar para um diretório local**:
  ```bash
  lcd /path/to/local/directory
  ```

- **Baixar um arquivo do servidor para o cliente**:
  ```bash
  get remote_file
  ```

- **Enviar um arquivo do cliente para o servidor**:
  ```bash
  put local_file
  ```

- **Sair da sessão SFTP**:
  ```bash
  exit
  ```

---

### **Passo 5: Testar o SFTP com Comando de Linha**

Além de interagir com o cliente SFTP interativamente, você pode usar o comando `sftp` de forma não interativa para transferir arquivos. Por exemplo:

```bash
sftp username@hostname_or_ip:/remote/path/to/file /local/path/to/destination
```

Este comando transferirá um arquivo do servidor remoto para o destino local.

---

### **Passo 6: Monitorar Logs de Acesso (Opcional)**

Se você precisar monitorar as transferências de arquivos ou atividades de login no servidor, pode verificar os logs do SSH:

```bash
sudo tail -f /var/log/auth.log
```

Os logs contêm informações sobre tentativas de login e atividades de transferência realizadas através do SFTP.

---

### **Conclusão**

Com o **SFTP**, você tem uma maneira segura de transferir arquivos entre sistemas Linux, aproveitando a criptografia fornecida pelo SSH. Ele é uma excelente alternativa ao TFTP, especialmente em ambientes onde a segurança é uma prioridade.

Se você seguiu as etapas corretamente, deve ser capaz de transferir arquivos de forma segura usando o SFTP.