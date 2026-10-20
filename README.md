# Atualização do site TEU7VD

Este guia explica como adicionar fliers, fotos, vídeos e atividades sem quebrar a organização do site. O site é estático: os arquivos ficam em `assets/`, enquanto os textos, datas e a associação entre cada arquivo e seu lugar no site são configurados em `index.html`.

## Estrutura do repositório

```text
.
├── index.html                 # conteúdo, agenda e estrutura das seções
├── assets/                    # logos, fliers, fotos e vídeos
└── .github/workflows/         # publicação automática no GitHub Pages
```

O site é publicado pelo GitHub Pages sempre que uma alteração chega à branch `main`. O endereço publicado é `https://dihjanssencoelho-svg.github.io/siteTEU7VD/`.

## Antes de inserir arquivos

- Use nomes curtos, em minúsculas, sem espaços ou acentos. Exemplo: `gira-caboclos-2026-11-06.jpg`.
- Coloque imagens e vídeos dentro de `assets/` e use o caminho começando por `assets/` no código.
- Para fliers, prefira JPG ou WebP. Para fotos, JPG ou WebP. Para vídeos, MP4 com vídeo H.264 e áudio AAC costuma funcionar nos navegadores.
- Comprima os arquivos antes de enviar. Imagens com até cerca de 1–2 MB e vídeos curtos com tamanho reduzido carregam melhor em celulares.
- Não apague ou renomeie um arquivo já usado sem atualizar todas as referências a ele em `index.html`.
- Confirme autorização para publicar fotos e vídeos de pessoas identificáveis.

## Onde cada conteúdo aparece

### Fliers de cursos e atividades

1. Envie o arquivo para `assets/`.
2. Em `index.html`, localize `const flyers = [...]`.
3. Inclua uma entrada com o título e o caminho do arquivo, por exemplo:

```js
['Curso de exemplo', 'assets/curso-exemplo.jpg']
```

O carrossel **Fliers e atividades** usa os fliers cadastrados na página. O quadro de cursos também usa essa lista para mostrar as imagens quando aberto. Para que o flier apareça como cartão no carrossel, a imagem precisa ser adicionada à estrutura HTML de `#flyerCarousel`, com `src="assets/arquivo.jpg"`, texto alternativo (`alt`) e legenda (`caption`). Use os cartões existentes como modelo e mantenha a classe `slide`.

### Fotos e vídeos de eventos ou giras

1. Envie o arquivo para `assets/` com nome que identifique o evento e a data, por exemplo `gira-eres-2026-10-11.mp4`.
2. Para exibir uma foto, use uma imagem com legenda no carrossel ou no conteúdo do registro. Para vídeo, use este formato:

```html
<article class="media-card">
  <video controls preload="metadata" playsinline>
    <source src="assets/gira-eres-2026-10-11.mp4" type="video/mp4">
    Seu navegador não reproduz este vídeo.
  </video>
  <p>Gira de Erês · 11 de outubro de 2026</p>
</article>
```

3. No calendário de **Registros**, localize a lista `events` dentro da opção `calendar` de `openModal`. Adicione a data como chave no formato `AAAA-MM-DD` e informe nome, data por extenso e caminho do vídeo:

```js
'2026-10-11': {
  name: 'Gira de Erês',
  date: '11 de outubro de 2026',
  video: 'assets/gira-eres-2026-10-11.mp4'
}
```

O calendário de registros mostra o vídeo ao clicar na data. Para a foto ou vídeo também aparecer diretamente na seção **Registros**, adicione um `article.media-card` como o exemplo acima dentro do carrossel `#recordsCarousel`.

### Datas de giras, cursos, aulas e eventos na agenda

No início do script de `index.html`, localize `const agendaEvents = { ... }`. Cada data usa o formato `AAAA-MM-DD`; a atividade pode ter um destes tipos:

- `gira` — gira. Para evento fechado, acrescente `closed: true`.
- `course` — curso.
- `class` — aula ou atividade regular.
- `development` — aula de desenvolvimento mediúnico.
- `event` — outro evento.

Exemplos:

```js
'2026-11-06': [{ name: 'Gira de Caboclos', kind: 'gira' }],
'2026-11-15': [{ name: 'Gira fechada - Homenagem à Umbanda', kind: 'gira', closed: true }],
'2026-12-13': [{ name: 'Confraternização da família TEU7VD', kind: 'event' }]
```

Uma data pode ter mais de uma atividade. Nesse caso, mantenha todas dentro da lista:

```js
'2026-11-07': [
  { name: 'Curso de exemplo', kind: 'course' },
  { name: 'Aula de exemplo', kind: 'class' }
]
```

As datas dessa lista alimentam os calendários e o carrossel de próximas giras. O calendário e os cartões usam o nome da atividade para mostrar o destaque e o símbolo. Datas futuras fora do mês vigente seguem as regras atuais do site e avisam que a agenda de reservas ainda pode mudar.

## Atualizar pelo GitHub em qualquer computador

O repositório no GitHub é a cópia compartilhada do site. Assim, uma pessoa pode atualizar o conteúdo usando o navegador de qualquer computador com acesso ao repositório; não precisa usar a máquina onde o site foi criado.

1. Entre em [github.com/dihjanssencoelho-svg/siteTEU7VD](https://github.com/dihjanssencoelho-svg/siteTEU7VD) e faça login.
2. Abra a pasta `assets` e escolha **Add file → Upload files** para enviar fotos, fliers e vídeos. Confirme que os arquivos foram adicionados dentro de `assets/`.
3. Para alterar textos, agenda ou referências dos arquivos, abra `index.html`, escolha o ícone de edição e faça as alterações seguindo os exemplos deste guia.
4. Revise a alteração e use **Commit changes**. Para uma atualização simples e urgente, pode gravar diretamente na branch `main`. Para revisão por outra pessoa, crie uma branch e envie um *pull request* para `main`.
5. Depois do commit ou da aprovação do *pull request*, abra a aba **Actions** do repositório. Aguarde o fluxo de publicação ficar verde e acesse o site publicado para conferir as imagens e datas.

### Trabalho com várias pessoas

- Combine quem vai atualizar cada atividade e peça para a equipe sempre enviar os arquivos ao repositório compartilhado.
- Prefira uma branch e um *pull request* por atualização. Isso permite revisar nomes, datas e arquivos antes da publicação.
- Evite editar o mesmo `index.html` ao mesmo tempo. Se houver conflito, uma pessoa deve comparar as alterações e juntar as duas antes de publicar.
- O histórico de commits mostra quem mudou cada conteúdo e permite localizar versões anteriores.

## Conferência antes da publicação

- A data está no formato `AAAA-MM-DD` e corresponde ao ano correto?
- O caminho e o nome do arquivo coincidem exatamente com o arquivo dentro de `assets/`?
- A atividade está na seção certa e tem uma legenda clara?
- O flier, a foto ou o vídeo abre no site em computador e celular?
- O fluxo de **Actions** concluiu com sucesso?

> Observação: o site não possui atualmente um painel administrativo. A edição pelo navegador do GitHub é a alternativa compartilhada já disponível; automatizar um formulário/painel para publicar conteúdo exigiria implementar e hospedar um CMS ou serviço com armazenamento.
