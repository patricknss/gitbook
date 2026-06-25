# Persistencia: NHibernate x Dapper

Guia de decisao para escolher a estrategia de persistencia no backend.

## Quando usar NHibernate

- Fluxos de escrita e agregados de dominio.
- Entidades com comportamento e relacoes mapeadas.
- Cenarios que exigem unidade de trabalho e tracking.

## Quando usar Dapper

- Consultas de leitura otimizadas.
- Relatorios e projecoes especificas.
- SQL direto com necessidade de performance ou controle fino.

## Regra pratica

- **Escrita**: priorize NHibernate.
- **Leitura complexa/otimizada**: considere Dapper.
- Evite misturar sem criterio no mesmo caso de uso.

## Transacao

- Abrir e fechar transacao na camada de Aplicacao.
- `Commit` apenas no caminho de sucesso.
- `Rollback` em qualquer falha.
- Dominio nao deve abrir transacao.

## Anti-patterns comuns

1. Controller chamando repositorio diretamente.
2. Regra de negocio embutida no SQL de repositorio.
3. Excesso de joins para montar resposta de escrita.
4. Falta de indice em consultas Dapper de alto volume.

## Checklist de implementacao

1. Contrato no Dominio criado (`I...Repositorio`).
2. Implementacao na Infra definida (NH/Dapper).
3. Transacao orquestrada na Aplicacao.
4. DTO de leitura/escrita separado quando necessario.
5. Testes cobrindo caminho feliz e falha.

## Veja tambem

- [NHibernate](../../bibliotecas/nhibernate.md)
- [Dapper](../../bibliotecas/dapper.md)
- [Arquitetura DDD Autoglass](../ddd/arquitetura-autoglass-ddd.md)
