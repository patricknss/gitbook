---
description: Anexar foto manualmente
---

# Exemplo prático

### Entrada do fluxo

Para anexar uma foto manualmente, o sistema recebe:

* ID da vistoria
* ID do passo
* arquivo
* comentário

### Regras de negócio

* O comentário é obrigatório
* O arquivo deve conter no máximo 100000 tb
* A vistoria não pode estar aprovada
* A vistoria não pode estar reprovada
* O sistema deve salvar o arquivo
* O sistema deve salvar o caminho do arquivo
* O sistema deve criar histórico
* O sistema pode alterar status dependendo das fotos obrigatórias

### Separando por camada

#### Controller

Recebe a requisição

POST `/vistorias/{id}/manualmente`

Recebe

* arquivo
* comentario
* id do passo

Depois chama o application service

#### Application Service

Executa o caso de uso

* Busca a vistoria
* Verifica se ela existe
* Valida se o comentário foi enviado
* Valida o tamanho do arquivo
* Chama o domínio pra verificar se o status permite anexo
* Envia o arquivo para o serviço de arquivos ou S3
* Salva os dados da foto
* Cria o histórico
* Retorna sucesso

#### Domínio

Valida a regra principal

A vistoria pode receber foto manualmente nesse status?

Se estiver aprovada, reprovada etc lança exceção

#### Repositorio

Busca e salva dados

* busca vistoria
* salva foto
* salva historico
* atualiza status, se necessário

#### Infra

Faz a ligação externa

* envia arquivo pro S3 ou serviço de arquivos
* retorna o caminho do arquivo salvo

***

## Minha explicação

Quando o usuário clica pra anexar uma foto, o Angular apenas monta a tela e envia os dados.

O Controller recebe a requisição, mas ele não deveria decidir tudo

O application service organiza o fluxo

O domínio valida se aquela ação pode acontecer

O repositório busca e salva os dados

A infra cuida das integrações externas tipo salvar o arquivo no S3

A regra mais importante é que o backend precisa garantir o comportamento correto, porque o front pode ser burlado.
