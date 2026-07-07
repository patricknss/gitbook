---
description: Regra de negócio, backend e responsabilidade das camadas
---

# Dia 1

### O que é uma regra de negócio?

Regra de negócio é uma regra que define como o sistema deve se comportar de acordo com o funcionamento real da empresa

Ela não exige porque o você (quem está codando) quis. Ela existe porque o processo da empresa exige.

Exemplo:\
Em uma vistoria, não pode anexar foto manualmente se a vistoria já estiver aprovada, reprovada etc\
Essa regra existe porque esses status representam estados finais ou estados em que o processo não deveria mais receber alteração manual.

### Porque regra de negócio não deve ficar só no Angular?

Porque o front pode ser burlado kkkk

Mesmo que o botão esteja desabilitado na tela, alguém poderia ainda chamar a API diretamente pelo Postman, pelo navegador ou por outro sistema

Por isso o front ajuda na experiência de usuário mas quem precisa garantir a regra de verdade é o backend

### Responsabilidade de cada camada

#### Controller

O controller recebe a requisicao HTTP\
Aqui ficam os endpoints

Ele não deve conter regra pesada de negócio

Exemplo:\
Receber o ID da vistoria, o arquivo, o comentário e chamar o service responsável

#### Application Service

Aqui é a camada de aplicação\
Ela coordena o caso de uso

Ele organiza o fluxo completo

* buscar a vistoira
* validar entrada
* cahmar regras do dominio
* salvar arquivo
* salvar dados
* criar histórico
* retornar resposta

Ele é o organizador do processo

#### Dominio

Representa as regras principais do sistema

É nele que ficam as decisões importantes do sistema, tipo

* pode anexar foto?
* pode mudar status?
* pode concluir a vistoria?
* pode reenviar foto?
* pode aprovar ou reprovar?

O domínio protege o sistema de estados inválidos

#### Repositorio

Busca e salva dados

Ele não deveria decidir regra de negócio

Exemplo:

* buscar vistoria por ID
* salvar foto
* salvar histórico
* consultar fotos obrigatórias

#### Infra

A infra conversa com coisas externas

* banco
* aws s3
* aws sqs
* redis
* api externa
* serviço de arquivos











