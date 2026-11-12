# Fale Conosco — configuração administrativa

## Componentes

- Página pública: `/fale-conosco.html`
- Painel administrativo: `/admin/manifestacoes.html`
- Projeto Supabase isolado: `TEU7VD-FaleConosco`
- Tabelas: `public.manifestacoes`, `public.manifestacoes_historico` e `private.administradores`

## Segurança implementada

- RLS habilitado nas três tabelas.
- Papel público (`anon`) pode inserir manifestação, mas não consultar, listar, editar ou excluir registros.
- Somente usuários autenticados cadastrados em `private.administradores` podem consultar e atualizar manifestações.
- O histórico é acessível apenas aos administradores autorizados.
- Não há coleta de IP ou identificador do navegador pelo formulário.
- A chave publishable é usada no navegador; nenhuma chave service-role é publicada.
- O protocolo é gerado no navegador e gravado junto à manifestação. Ele não concede acesso ao registro.

## Ativar o primeiro administrador

A conta administrativa ainda precisa ser criada e autorizada. Depois de receber o e-mail institucional/pessoal que será usado pelo responsável:

1. No Supabase, abra Authentication → Users e convide/crie o usuário com esse e-mail.
2. Confirme a conta e defina uma senha forte.
3. No SQL Editor, execute a instrução abaixo substituindo o e-mail pelo endereço exato do usuário:

```sql
insert into private.administradores (user_id)
select id from auth.users
where lower(email) = lower('EMAIL_DO_ADMIN')
on conflict (user_id) do nothing;
```

4. Acesse `https://dihjanssencoelho-svg.github.io/siteTEU7VD/admin/manifestacoes.html` e entre com o e-mail e senha cadastrados.
5. Confirme que a lista de manifestações abre. Contas que não estiverem na tabela privada não terão acesso aos dados.

Para revogar o acesso, remova o usuário da lista:

```sql
delete from private.administradores
where user_id = (select id from auth.users where lower(email) = lower('EMAIL_DO_ADMIN'));
```

## Operação do painel

Cada registro permite alterar a situação (Recebida, Em análise, Respondida ou Encerrada) e registrar uma providência ou resposta. Cada anotação é gravada em `manifestacoes_historico`, com autor autenticado e data/hora.

## Limites e próximos reforços

A publicação inicial usa o plano Free e uma chave publishable. Antes de divulgar amplamente o formulário, recomenda-se ativar proteção antispam (por exemplo, Cloudflare Turnstile com validação server-side) e definir política de retenção das manifestações. O anonimato no formulário significa que não são solicitados nome/contato e que a tabela não guarda IP; isso não equivale a uma promessa de anonimato absoluto sobre registros técnicos de infraestrutura.
