# Painel administrativo do TEU7VD

Este site continua hospedado no GitHub Pages. O painel administrativo é o Decap CMS e fica em `/admin/`. O conteúdo editável está em `content/site.json`; imagens e vídeos enviados pelo painel ficam em `assets/uploads/`. Quando uma alteração é aprovada e chega à branch `main`, o GitHub Actions atualiza o site publicado.

## Acesso ao painel

Endereço: <https://dihjanssencoelho-svg.github.io/siteTEU7VD/admin/>

O acesso é feito com uma conta GitHub autorizada a escrever no repositório `dihjanssencoelho-svg/siteTEU7VD`. Uma pessoa administradora do repositório deve conceder a permissão **Write** às pessoas que poderão usar o painel. Não compartilhe senhas, tokens ou códigos de autenticação.

> **Configuração inicial necessária:** antes do primeiro login funcionar, uma pessoa responsável pelo repositório precisa configurar o provedor OAuth do GitHub e trocar o endereço de exemplo `https://CONFIGURAR-WORKER-OAUTH.workers.dev` em `admin/config.yml` pela URL real do serviço OAuth. Veja a seção **Configuração inicial do acesso** abaixo. O painel e as coleções já estão preparados, mas o GitHub Pages sozinho não pode guardar o segredo OAuth.

## Configuração inicial do acesso

O Decap CMS usa o GitHub para gravar as alterações. Como o site é estático, o login OAuth precisa de um pequeno serviço seguro separado. A configuração abaixo usa um Cloudflare Worker para esse login; o site continua no GitHub Pages.

1. Uma pessoa responsável deve criar ou usar uma conta Cloudflare e criar uma aplicação de autenticação para o painel. Uma implementação comunitária compatível com Decap CMS e Cloudflare Workers está documentada em [decap-cms-github-oauth-provider-cloudflare](https://github.com/ottmartens/decap-cms-github-oauth-provider-cloudflare). Revise o código e as instruções do projeto antes de conectá-lo à conta GitHub do templo.
2. Crie a aplicação GitHub indicada pelo provedor. Configure a página inicial como `https://dihjanssencoelho-svg.github.io/siteTEU7VD/` e a URL de callback como `https://NOME-DO-WORKER.SEU-USUARIO.workers.dev/callback` (use a URL exata que o Worker informar).
3. Implante o provedor no Cloudflare seguindo as instruções dele. Guarde o **Client ID** e o **Client Secret** em variáveis secretas do Worker/Cloudflare. Nunca coloque o Client Secret em `index.html`, `config.yml`, `content/site.json` ou em uma mensagem pública.
4. Depois da implantação, copie o endereço base do Worker, por exemplo `https://teu7vd-cms-auth.seu-usuario.workers.dev`.
5. Em `admin/config.yml`, substitua somente `https://CONFIGURAR-WORKER-OAUTH.workers.dev` pelo endereço base real. Grave e publique essa alteração no repositório.
6. No GitHub, proteja a branch `main` em **Settings → Rules → Rulesets** (ou **Settings → Branches**, conforme a interface disponível). Exija pull request, mas não exija aprovação de outra conta quando houver apenas uma pessoa responsável pelo repositório; assim, essa pessoa poderá revisar e integrar as alterações.
7. Em **Settings → Collaborators** (ou nas configurações de acesso da organização), conceda permissão **Write** às contas autorizadas a usar o painel. Não conceda acesso administrativo completo ao repositório se a pessoa só precisa cadastrar conteúdo.
8. Abra o endereço `/admin/`, entre com uma conta autorizada e confirme que as coleções **Flyers e atividades**, **Giras, cursos, aulas e eventos**, **Fotos e vídeos de eventos** e **Aulas de desenvolvimento mediúnico** aparecem.

O CMS usa o fluxo editorial. Ao salvar, a alteração fica em uma proposta de publicação para revisão; uma pessoa responsável aprova e integra a proposta à `main`. Só então o fluxo do GitHub Actions publica as mudanças. O Decap documenta este modo de rascunho e revisão em [Editorial Workflows](https://decapcms.org/docs/editorial-workflows/).

## Passo a passo para enviar um flyer

1. Acesse `/admin/` e faça login com a conta GitHub autorizada.
2. Abra **Conteúdo do site → Agenda, flyers e registros**.
3. Em **Flyers e atividades**, clique em **Add Flyers e atividades** (ou abra um item existente para substituir os dados).
4. Preencha o título, envie a imagem no campo **Imagem do flyer**, escreva um texto alternativo que descreva a imagem e uma legenda curta.
5. Salve como rascunho e mova a proposta para revisão.
6. Uma pessoa revisora confere a imagem e a legenda, aprova e integra a proposta. Aguarde o fluxo **Publicar site no GitHub Pages** ficar verde.

O flyer aparece no carrossel **Flyers e atividades** e na janela **Cursos ministrados**. Use uma imagem JPG, PNG ou WebP legível em celular. O limite de envio configurado para um flyer é 10 MB.

## Passo a passo para cadastrar uma gira, curso, aula ou evento

1. Na mesma coleção, abra **Giras, cursos, aulas e eventos** e adicione um item.
2. Informe a data no formato `AAAA-MM-DD`, por exemplo `2026-11-06`.
3. Escreva o nome que será mostrado no site.
4. Escolha o tipo correto: **Gira**, **Curso**, **Aula**, **Desenvolvimento mediúnico** ou **Outro evento**.
5. Marque **Gira fechada ao público** somente quando a gira realmente não for aberta a visitantes.
6. Se o nome ficar longo no calendário, informe um **Nome curto na agenda**. Use **Situação ou observação** para informar, por exemplo, que uma turma já começou ou que a data ainda será confirmada.
7. Salve como rascunho, envie para revisão e aguarde a aprovação e a publicação.

As datas cadastradas alimentam os calendários e o carrossel de próximas giras. As aulas semanais de desenvolvimento mediúnico são geradas automaticamente de acordo com a configuração de dias, horários e situação da turma. Para alterar esses dados, edite a coleção **Aulas de desenvolvimento mediúnico**. As quartas e quintas já iniciadas devem continuar com a situação de turma iniciada; a turma de sábado pode ficar como aguardando início/data a confirmar.

## Passo a passo para enviar fotos e vídeos de eventos

1. Abra **Fotos e vídeos de eventos** e adicione um item.
2. Informe o título. Se quiser que o evento também apareça no calendário de registros, preencha a data no formato `AAAA-MM-DD`. Deixe em branco para um registro sem data definida.
3. Escolha **Foto** ou **Vídeo** no campo tipo de arquivo.
4. Envie ou selecione o arquivo no campo **Foto ou vídeo**. O limite configurado para um registro é 25 MB.
5. Salve como rascunho, envie para revisão e aguarde a aprovação e publicação.

Fotos e vídeos aprovados aparecem na seção **Registros**. Se o item tiver data, o calendário de registros também destacará o dia e permitirá abrir a mídia correspondente. Para vídeos, use MP4 (H.264/AAC) sempre que possível; vídeos grandes devem ser reduzidos antes do envio. O repositório é destinado a arquivos leves. Para acervos de vídeo grandes, será necessário configurar armazenamento/CDN externo e adaptar o campo de mídia.

## Alterar uma publicação ou corrigir um erro

Abra a coleção correspondente, selecione o item, ajuste os campos e salve uma nova proposta. Para retirar uma atividade, remova o item da coleção de agenda. Para substituir uma imagem ou vídeo, selecione o novo arquivo no campo de mídia e salve. Depois da aprovação, confira a aba **Actions** e abra o site em computador e celular.

## Onde os arquivos e os dados ficam

```text
admin/index.html       # interface do Decap CMS
admin/config.yml       # coleções e integração com GitHub/OAuth
content/site.json      # flyers, agenda, registros e aulas semanais
assets/uploads/        # mídia enviada pelo painel
.github/workflows/     # publicação automática do Pages
```

Evite editar `content/site.json` manualmente enquanto outra pessoa estiver com uma proposta aberta. Não renomeie ou apague arquivos dentro de `assets/uploads/` sem primeiro corrigir o item que os utiliza.

## Problemas comuns

- **A tela `/admin/` abre, mas o login falha:** confirme que o endereço `base_url` em `admin/config.yml` corresponde ao Worker implantado e que o callback cadastrado na aplicação GitHub está correto.
- **A conta entra, mas não consegue salvar:** confirme que ela tem permissão **Write** no repositório.
- **Uma proposta não aparece no site:** verifique se foi aprovada e integrada à `main`; depois confira se o GitHub Actions concluiu a publicação.
- **Imagem ou vídeo não aparece:** confira se a proposta foi aprovada, se o item ainda referencia o arquivo enviado e se o formato/tamanho é suportado pelo navegador.
- **Duas pessoas alteraram o mesmo conteúdo:** peça a uma pessoa revisora para comparar as propostas e integrar uma de cada vez, evitando substituir dados mais recentes.

## Cuidados

- Não compartilhe o Client Secret OAuth, tokens ou códigos de login.
- Conceda acesso ao repositório apenas às pessoas que precisam editar ou revisar conteúdo.
- Antes de publicar fotos ou vídeos com pessoas identificáveis, confirme que há autorização para divulgação.
- O histórico do GitHub registra as alterações e permite localizar quem publicou cada atualização.

