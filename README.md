# Configurar o XAMPP para trabalhar com HTTPS e SSL no Windows

## Instalação do Certificado

1 - Descomentar as seguintes linhas no arquivo `C:\xampp\php\php.ini`:
```
extension=openssl
extension=php_openssl.dll
```
2 - Criar um arquivo com o nome `v3.ext` na pasta `C:\xampp\apache` com o seguinte conteúdo:
```
authorityKeyIdentifier=keyid,issuer
basicConstraints=CA:FALSE
keyUsage = digitalSignature, nonRepudiation, keyEncipherment, dataEncipherment
subjectAltName = @alt_names
[alt_names]
DNS.1 = localhost
DNS.2 = *localhost
```

3 - Abrir o arquivo `C:\xampp\apache\makecert.bat` e substituir a linha conforme abaixo:

- <s>de: `bin\openssl x509 -in server.csr -out server.crt -req -signkey server.key -days 365`</s>
- para: `bin\openssl x509 -in server.csr -out server.crt -req -signkey server.key -days 365 -sha256 -extfile v3.ext`

4 - Executar o arquivo `C:\xampp\apache\makecert.bat` como administrador e preencher conforme abaixo:
- Enter PEM pass prhase: digite uma senha
- Verifying - Enter PEM pass phrase: digite novamente a senha
- Enter até chegar o campo Common Name, preencha com: localhost
- Enter até chegar o campo Enter pass phrase for privkey.pem: digite a senha criada no início

5 - Clique com o botão direito no arquivo `C:\xampp\apache\conf\ssl.crt\server.crt` e instale o certificado:
- Local do Repositório: Usuário Atual
- Repositório de Certificados: Autoridades de Certificação Raiz Confiáveis
- Confirme se o certificado foi instalado indo em Painel de Controle > Gerenciar Certificados de Usuário > Autoridades de Certificação Raiz Confiáveis > Certificados

6 - Acessar o arquivo `C:\xampp\apache\conf\extra\httpd-ssl.conf` e alterar/incluir as linhas abaixo para:
- SSLCertificateFile "conf/ssl.crt/server.crt"
- SSLCertificateKeyFile "conf/ssl.key/server.key"

fonte: https://www.youtube.com/watch?v=g8jxVI18WZ0

## Configurações importantes para utilizar SSL com Virtual Host:
1 - Acessar o arquivo `C:\xampp\apache\conf\extra\httpd-vhosts.conf` e alterar/incluir as linhas abaixo para:
- SSLCertificateFile C:/xampp/apache/conf/ssl.crt/server.crt
- SSLCertificateKeyFile C:/xampp/apache/conf/ssl.key/server.key

# Dica bônus
- Se você estiver utilizando Virtual Host e ele não estiver funcionando, acesse o arquivo `C:\xampp\apache\conf\extra\httpd-ssl.conf` e altere o Listen para a mesma porta do virtual Host 
