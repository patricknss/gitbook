# Amazon Cognito

Amazon Cognito é um serviço de autenticação e gestão de usuários.

## Quando usar

- Login e cadastro de usuários.
- Integração com provedores sociais (Google, etc.).
- Emissão de tokens JWT para APIs.

## Conceitos

- **User Pool**: diretório de usuários.
- **App Client**: aplicação autorizada a autenticar.
- **Identity Pool**: acesso temporário a recursos AWS.

## Exemplo de fluxo

1. Usuário faz login no frontend.
2. Cognito valida credenciais no User Pool.
3. Aplicação recebe `id_token` e `access_token`.
4. API valida o JWT antes de liberar acesso.

## Exemplo de payload de token (resumo)

```json
{
  "sub": "8df2f9b2-xxxx-xxxx-xxxx-7ad7f0f8f111",
  "email": "usuario@empresa.com",
  "token_use": "id",
  "exp": 1782300000
}
```

## Erros comuns

- Usar token sem validar assinatura/expiração na API.
- Misturar responsabilidade de autenticação e autorização.
- Expor secrets do App Client no frontend.
