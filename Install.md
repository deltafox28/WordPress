# Execute a atualização do Ubuntu 22.04:

## Em primeiro lugar, execute o comando de atualização do sistema para garantir que todos os pacotes em nosso sistema estejam atualizados e também que o cache do índice de pacotes APT esteja em seu estado mais recente.
```sh
sudo apt update && sudo apt upgrade
```

# Instale Apache e PHP para WordPress

## Precisamos de um servidor web Apache e linguagem de programação PHP para configurar o WordPress CMS, vamos instalar ambos nesta etapa.
```sh
sudo apt install apache2
```

## Assim que a instalação do Apache estiver concluída, habilite e inicie seu serviço.
```sh
sudo systemctl enable apache2
```

## Verifique o estado.
```sh
systemctl status apache2
```

## endereço IP do servidor com seu endereço real
```sh
http://server-ip-address
```

## Instale a versão 8 do PHP.
```sh
sudo apt install -y php php-{common,mysql,xml,xmlrpc,curl,gd,imagick,cli,dev,imap,mbstring,opcache,soap,zip,intl}
```

## Para verificar a versão após concluir o comando acima, use:
```sh
php -v
```

# Instale MariaDB ou MySQL.

## Podemos usar MariaDB ou MySQL Database Server no Ubuntu 22.04 para armazenar os dados gerados pelo WordPress CMS. Aqui estamos usando o MariaDB Server.
```sh
sudo apt install mariadb-server mariadb-client
```

## Habilite, inicie e verifique o status do serviço:
```sh
 sudo systemctl enable --now mariadb
```

## Verificar:
```
shsystemctl status mariadb
Ctrl+C
```

## Proteja sua instalação de banco de dados:
Para proteger nossa instância do banco de dados, execute o comando fornecido:
```sh
 sudo mysql_secure_installation
```

## Saída. As perguntas dadas serão feitas pelo sistema, o exemplo de respostas também é dado abaixo:
```sh
 Enter current password for root (enter for none): Press ENTER
Set root password? [Y/n]: Y
New password: Set-your-new-password
Re-enter new password: Set-your-new-password
Remove anonymous users? [Y/n] Y
Disallow root login remotely? [Y/n] Y
Remove test database and access to it? [Y/n] Y
Reload privilege tables now? [Y/n] Y
```
# Crie banco de dados para WordPress

## Faça login no seu servidor de banco de dados usando a senha que você definiu para o usuário root dele.
```sh
sudo mysql -u root -p
```

## Siga o comando para criar um novo banco de dados. No entanto, não se esqueça de substituir new_user por qualquer nome que você queira dar ao seu usuário do banco de dados e, da mesma forma, new_db por um nome para o banco de dados e your_password para a senha.
```sh
CREATE USER 'new_user'@'localhost' IDENTIFIED BY 'your_password';
CREATE DATABASE new_db;
GRANT ALL PRIVILEGES ON new_db.* TO 'new_user'@'localhost';
FLUSH PRIVILEGES;
Exit;
```

# Instale o WordPress no Ubuntu 22.04

## Os arquivos para configurar o WordPress precisam ser baixados manualmente e podemos fazer isso usando o terminal de comando. Aqui estão os comandos a seguir:
```sh
sudo apt install wget unzip
```

## Baixar WordPress:
```sh
wget https://wordpress.org/latest.zip
```

## Extrair arquivo:
```sh
sudo unzip latest.zip
```

## Mova-o para a pasta da web: 
```sh
sudo mv wordpress/ /var/www/html/
```

## Remova os arquivos baixados para liberar espaço:
```sh
sudo rm latest.zip
```

## Alterar permissão de arquivo
```sh
sudo chown www-data:www-data -R /var/www/html/wordpress/
sudo chmod -R 755 /var/www/html/wordpress/
```
 
#  Configure o Apache no Ubuntu 22.04

## Em seguida, habilite os módulos e o arquivo de configuração Vhost do seu servidor Apache para garantir que ele sirva os arquivos PorcessWire CMS sem nenhum erro. Crie um arquivo de configuração para WordPress
```sh
sudo nano /etc/apache2/sites-available/wordpress.conf
```

## Copie e cole as seguintes linhas:
```sh
<VirtualHost *:80>

ServerAdmin admin@example.com

DocumentRoot /var/www/html/wordpress
ServerName example.com
ServerAlias www.example.com

<Directory /var/www/html/wordpress/>

Options FollowSymLinks
AllowOverride All
Require all granted

</Directory>

ErrorLog ${APACHE_LOG_DIR}/error.log
CustomLog ${APACHE_LOG_DIR}/access.log combined

</VirtualHost>
```

## Salve o arquivo pressionando Ctlr+O , pressionando a tecla Enter e saindo usando Ctrl+X . Ativar host virtual
```sh
sudo a2ensite wordpress.conf
```

## Ativar módulo de reescrita
```sh
sudo a2enmod rewrite
```

## sudo a2enmod rewrite
```sh
sudo a2dissite 000-default.conf
```

## Reinicie o servidor Apache para aplicar as alterações:
```sh
sudo systemctl restart apache2
```

# Configuração da interface web do WordPress CMS
## Depois de seguir as etapas acima, abra o navegador do sistema que pode acessar o endereço IP do servidor do sistema onde você instalou o WordPress. E apontá-lo como segue:
```sh
http://your-server-ip-address
```
