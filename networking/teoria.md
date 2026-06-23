---
icon: book-open
---

# Teoria

## Soquetes de rede <a href="#network-sockets" id="network-sockets"></a>

**Socket** - é um **ponto final** de um link de comunicação bidirecional entre dois programas em execução na rede. Os sockets têm dois estados principais: estão **conectados** e facilitando uma comunicação de rede contínua ou aguardando **uma** conexão de entrada para se conectar a eles. O socket de escuta é chamado de servidor, e o socket que solicita uma conexão com o socket de escuta é chamado de cliente. Você pode usar **o comando netstat** para gerenciar e descobrir seus próprios sockets, para que e onde eles são usados. A seção "Internet Ativa" lista as conexões de rede que são (ou serão) estabelecidas com dispositivos externos. A seção "Domínio UNIX" lista as conexões que foram estabelecidas dentro do seu computador entre diferentes aplicativos, processos e elementos do sistema operacional.

**Estados do soquete TCP**

| Estado do Soquete | Explicação                                                                                                                           |
| ----------------- | ------------------------------------------------------------------------------------------------------------------------------------ |
| **LISTEN**        | Lado do servidor. Socket aguardando uma solicitação de conexão                                                                       |
| **SYN-SENT**      | Lado do cliente. O soquete fez uma solicitação de conexão e aguarda.                                                                 |
| **SYN-RECEIVED**  | Do lado do servidor. O soquete está aguardando uma confirmação de conexão após aceitar a solicitação.                                |
| **ESTABLISHED**   | Servidor e Cliente. Uma conexão funcional foi estabelecida entre o servidor e o cliente, permitindo a transferência de dados         |
| **FIN-WAIT-1**    | Servidor e Cliente. O soquete está aguardando uma solicitação de término ou o reconhecimento de uma solicitação de término anterior. |
| **FIN-WAIT-2**    | Servidor e Cliente. O soquete está aguardando uma solicitação de término                                                             |
| **CLOSE-WAIT**    | O soquete está aguardando uma confirmação de solicitação de término do usuário local                                                 |
| **CLOSING**       | Servidor e Cliente. O soquete está aguardando uma confirmação de solicitação de término do soquete remoto.                           |
| **LAST-ACK**      | Servidor e Cliente. O soquete está aguardando confirmação de término do soquete remoto.                                              |
| **TIME-WAIT**     | Servidor e Cliente. Servidor e Cliente. Verificando se a confirmação de término foi recebida                                         |
| **CLOSED**        | Sem conexão, o soquete está encerrado                                                                                                |

## Zona Desmilitarizada <a href="#dmz" id="dmz"></a>

**`DMZ`**, ou **Zona Desmilitarizada** , no contexto de redes de computadores, é uma **área segregada** que atua como um buffer entre uma rede interna confiável e uma rede externa não confiável, como a internet. Normalmente, ela contém servidores que precisam ser acessíveis pela internet, como servidores web ou de e-mail. Isso `DMZ`ajuda a aumentar a segurança ao isolar esses servidores da rede interna, reduzindo o risco de acesso não autorizado a informações confidenciais.

## SSL <a href="#ssl" id="ssl"></a>

**`SSL`**, ou **Secure Sockets Layer , é um protocolo de segurança da Internet** baseado em criptografia . Foi desenvolvido pela Netscape em **1995** com o objetivo de garantir privacidade, autenticação e integridade de dados nas comunicações pela Internet. **O SSL** é o antecessor da criptografia **TLS** moderna usada hoje.

### O que é um `SSL`certificado? <a href="#what-is-an-ssl-certificate" id="what-is-an-ssl-certificate"></a>

`SSL`só pode ser implementado por sites que possuam um certificado SSL (tecnicamente, um "certificado TLS"). Um `SSL`certificado é como um documento de identidade ou um crachá que comprova que alguém é quem diz ser. `SSL`Os certificados são armazenados e exibidos na web pelo servidor de um site ou aplicativo.

Uma das informações mais importantes em um `SSL`certificado é a chave pública do site. A chave pública possibilita a criptografia e a autenticação. O dispositivo do usuário visualiza a chave pública e a utiliza para estabelecer chaves de criptografia seguras com o servidor web. O servidor web também possui uma chave privada que é mantida em segredo; a chave privada descriptografa os dados criptografados com a chave pública.

### Quais são os tipos de certificados SSL? <a href="#what-are-the-types-of-ssl-certificates" id="what-are-the-types-of-ssl-certificates"></a>

Existem vários tipos diferentes de certificados SSL. Um certificado pode ser aplicado a um único site ou a vários sites, dependendo do tipo:

* **Domínio único** : um certificado de domínio único`SSL`se aplica a apenas um domínio (um "domínio" é o nome de um site, como www.cloudflare.com).
* **Curinga** : Assim como um certificado de domínio único, um certificado SSL curinga se aplica a apenas um domínio. No entanto, ele também inclui os subdomínios desse domínio. Por exemplo, um certificado curinga pode abranger www.cloudflare.com, blog.cloudflare.com e developers.cloudflare.com, enquanto um certificado de domínio único pode abranger apenas o primeiro.
* **Multidomínio** : como o nome indica, os certificados SSL multidomínio podem ser aplicados a vários domínios não relacionados.

## OpenSSL <a href="#openssl" id="openssl"></a>

**O OpenSSL é um kit de ferramentas de código aberto** amplamente utilizadoe **TLS (Transport Layer Security)** . Ele fornece um conjunto de funções e utilitários criptográficos que permitem a comunicação segura em uma rede de computadores. O OpenSSL é comumente usado para criar e gerenciar certificados SSL/TLS, gerar chaves criptográficas e executar diversas operações criptográficas.

Aqui estão alguns comandos básicos e comuns do OpenSSL no Linux:

1. Verifique a versão do OpenSSL:

```bash
openssl version
```

1. Gerar uma chave privada:

```bash
openssl genprsa -out private-key.pem 2048
```

1. Gerar uma chave pública a partir de uma chave privada:

```bash
openssl rsa -in private-key.pem -pubout -out public-key.pem
```

1. Gerar um certificado autoassinado:

```bash
openssl req -x509 -newkey rsa:2048 -keyout private_key.pem -out certificate.pem -days 365
```

1. Ver informações do certificado:

```bash
openssl x509 -in certificate.pem -text -noout
```

1. Criptografar/descriptografar um arquivo usando RSA:
   *   Criptografar:

       ```bash
       openssl rsautl -encrypt -inkey public_key.pem -pubin -in plaintext.txt -out encrypted_data.bin
       ```
   *   Descriptografar:

       ```
       openssl rsautl -decrypt -inkey private_key.pem -in encrypted_data.bin -out decrypted_data.txt
       ```
2. Hashing:
   *   Gerar hash MD5:

       ```bash
       openssl md5 file.txt
       ```
   *   Gerar hash SHA-256:

       ```bash
       openssl sha256 file.txt
       ```
