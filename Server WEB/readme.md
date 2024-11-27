Aqui está um guia para configurar um **servidor web** no Linux utilizando o **Apache** (para ambientes mais tradicionais) ou o **Nginx** (para desempenho otimizado). Ambos são servidores web populares e amplamente utilizados. Vou cobrir ambos os casos para você.

---

### **Configuração de um Servidor Web com Apache**

1. **Instalar o Apache**

   - **Debian/Ubuntu**:
     ```bash
     sudo apt update
     sudo apt install apache2
     ```

   - **CentOS/RHEL**:
     ```bash
     sudo yum install httpd
     ```

2. **Iniciar e Habilitar o Apache**

   Após a instalação, inicie o serviço Apache e habilite para iniciar automaticamente com o sistema:

   ```bash
   sudo systemctl start apache2   # Para Debian/Ubuntu
   sudo systemctl enable apache2  # Para Debian/Ubuntu

   sudo systemctl start httpd     # Para CentOS/RHEL
   sudo systemctl enable httpd    # Para CentOS/RHEL
   ```

3. **Verificar o Status do Apache**

   Para verificar se o Apache está em execução corretamente, use:

   ```bash
   sudo systemctl status apache2   # Debian/Ubuntu
   sudo systemctl status httpd     # CentOS/RHEL
   ```

   Você também pode acessar o servidor web localmente no navegador digitando `http://localhost/`. Se o Apache estiver funcionando corretamente, você verá a página de boas-vindas padrão do Apache.

4. **Configuração Básica do Apache**

   O arquivo de configuração principal do Apache é `/etc/apache2/apache2.conf` (Debian/Ubuntu) ou `/etc/httpd/conf/httpd.conf` (CentOS/RHEL). Você pode editar esse arquivo para personalizar o comportamento do servidor.

   Para editar o arquivo de configuração no Debian/Ubuntu:

   ```bash
   sudo nano /etc/apache2/apache2.conf
   ```

   Para editar no CentOS/RHEL:

   ```bash
   sudo nano /etc/httpd/conf/httpd.conf
   ```

5. **Testar o Servidor Web**

   Para testar a configuração do Apache e garantir que está funcionando, você pode usar o comando `apache2ctl` (Debian/Ubuntu) ou `httpd` (CentOS/RHEL):

   - **Debian/Ubuntu**:
     ```bash
     sudo apache2ctl configtest
     ```

   - **CentOS/RHEL**:
     ```bash
     sudo apachectl configtest
     ```

   Se a configuração estiver correta, você verá a mensagem `Syntax OK`.

---

### **Configuração de um Servidor Web com Nginx**

1. **Instalar o Nginx**

   - **Debian/Ubuntu**:
     ```bash
     sudo apt update
     sudo apt install nginx
     ```

   - **CentOS/RHEL**:
     ```bash
     sudo yum install nginx
     ```

2. **Iniciar e Habilitar o Nginx**

   Após a instalação, inicie o serviço Nginx e habilite para iniciar automaticamente com o sistema:

   ```bash
   sudo systemctl start nginx     # Debian/Ubuntu
   sudo systemctl enable nginx    # Debian/Ubuntu

   sudo systemctl start nginx     # CentOS/RHEL
   sudo systemctl enable nginx    # CentOS/RHEL
   ```

3. **Verificar o Status do Nginx**

   Para verificar se o Nginx está funcionando corretamente, use o comando:

   ```bash
   sudo systemctl status nginx
   ```

   Você também pode acessar o servidor web localmente no navegador digitando `http://localhost/`. Se o Nginx estiver funcionando corretamente, você verá a página de boas-vindas padrão do Nginx.

4. **Configuração Básica do Nginx**

   O arquivo de configuração principal do Nginx está localizado em `/etc/nginx/nginx.conf`. Você pode editá-lo para ajustar as configurações do servidor.

   Para editar o arquivo de configuração:

   ```bash
   sudo nano /etc/nginx/nginx.conf
   ```

   Além disso, a configuração dos sites do Nginx é feita nos arquivos localizados em `/etc/nginx/sites-available/` e `/etc/nginx/sites-enabled/`. O Nginx usa esses arquivos para configurar múltiplos sites e domínios.

5. **Testar o Servidor Web**

   Para testar a configuração do Nginx, use o seguinte comando:

   ```bash
   sudo nginx -t
   ```

   Se a configuração estiver correta, você verá a mensagem `nginx: configuration file /etc/nginx/nginx.conf test is successful`.

---

### **Hospedando um Site**

1. **Criar um Diretório de Conteúdo**

   Para ambos os servidores (Apache e Nginx), você precisa criar um diretório para armazenar os arquivos do site:

   ```bash
   sudo mkdir -p /var/www/html/meusite
   sudo chmod -R 755 /var/www/html/meusite
   ```

2. **Criar um Arquivo HTML de Teste**

   Crie um arquivo simples para testar o servidor web:

   ```bash
   sudo nano /var/www/html/meusite/index.html
   ```

   E adicione um conteúdo básico:

   ```html
   <html>
       <head><title>Bem-vindo ao Meu Site!</title></head>
       <body>
           <h1>Meu primeiro site no Apache/Nginx!</h1>
       </body>
   </html>
   ```

3. **Configurar o Servidor para Servir o Novo Site**

   - **Apache**: Edite o arquivo de configuração do site (no Debian/Ubuntu geralmente em `/etc/apache2/sites-available/000-default.conf`).

     ```bash
     sudo nano /etc/apache2/sites-available/000-default.conf
     ```

     Adicione ou modifique para o seguinte:

     ```apache
     <VirtualHost *:80>
         DocumentRoot /var/www/html/meusite
         ServerName meusite.local
     </VirtualHost>
     ```

     Para habilitar o site e reiniciar o Apache:

     ```bash
     sudo a2ensite 000-default.conf
     sudo systemctl restart apache2
     ```

   - **Nginx**: Crie um arquivo de configuração para o novo site em `/etc/nginx/sites-available/` e crie um link simbólico em `/etc/nginx/sites-enabled/`:

     ```bash
     sudo nano /etc/nginx/sites-available/meusite
     ```

     Adicione a seguinte configuração:

     ```nginx
     server {
         listen 80;
         server_name meusite.local;
         root /var/www/html/meusite;
         index index.html;
     }
     ```

     Crie o link simbólico e reinicie o Nginx:

     ```bash
     sudo ln -s /etc/nginx/sites-available/meusite /etc/nginx/sites-enabled/
     sudo systemctl restart nginx
     ```

---

### **Considerações Finais**

- **Apache**: Ideal para quem busca um servidor robusto, com uma vasta gama de módulos e configurações, muito utilizado em ambientes tradicionais.
- **Nginx**: Conhecido pelo alto desempenho, ideal para servir conteúdos estáticos de forma rápida e eficiente, sendo muito utilizado para balanceamento de carga e como proxy reverso.

Escolha o servidor web conforme suas necessidades de desempenho e características do projeto. Ambos são confiáveis e amplamente utilizados no mercado.