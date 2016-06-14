# Zabbix HTML Mail

### Instalação

#### Instale o sendEmail

Faça o download em:

http://caspian.dotconf.net/menu/Software/SendEmail/

Extraia o conteudo de sendEmail-v1.56.tar.gz

```
# tar -vzxf sendEmail-v1.56.tar.gz
```

Acesse o diretório extraido:

```
# cd sendEmail-v1.56
```

Copie o arquivo sendEmail para /usr/bin

```
# cp sendEmail /usr/bin/
```

Lembre-se de dar as devidas permissões para o usuário Zabbix

#### Crie um script no diretório do alertcripts do zabbix

Para descobrir o caminho do seu alertscripts use o seguinte código:
```
$ cat /etc/zabbix/zabbix_server.conf | grep AlertScriptsPath=
```

Crie um arquivo zabbix_email.sh e cole o seguinte conteúdo:

```
#!/bin/sh
export smtpemailfrom=EMAIL DO REMTENTE
export zabbixemailto="$1"
export zabbixsubject="$2"
export zabbixbody="$3"
export smtpserver=ENDEREÇO OU IP DO SERVIDOR SMTP

/usr/bin/sendEmail -f $smtpemailfrom -t "$zabbixemailto" -u "$zabbixsubject" \
-m "$zabbixbody" -s $smtpserver:25 -o message-charset=utf-8  \
-o message-content-type=html >> /var/log/sendEmail.log
```

OBS: Caso seu servidor de SMTP exija SSL ou TLS será necessário fazer algumas alterações neste script, para tal olhe a documentação do site do sendEmail.

#### Configurando o E-mail no Zabbix

Entre no Zabbix

Clique em Administração -> Tipos de Mídias -> Criar tipo de Mídia

Coloque o Nome: Zabbix sendEmail
Tipo: Script
Nome Script: zabbix_email.sh
Clique em Adicionar 

Depois cole o contéudo do arquivo [email.html](email.html) na mensagem padrão da ação desejada.

Caso deseje pode modificar o html para tirar informações desnecessárias na menssagem de recuperação
