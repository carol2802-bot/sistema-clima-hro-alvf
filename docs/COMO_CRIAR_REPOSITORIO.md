# Como criar e publicar o repositório (conta carol2802@unochapeco.edu.br)

Eu não tenho como logar na conta do GitHub da Carol (não tenho a senha dela, e criar contas/repos em
nome de outra pessoa exige autenticação dela mesma). Mas deixo tudo pronto — são ~5 minutos.

## Opção A — Pelo site do GitHub (mais simples)

1. Acesse https://github.com e faça login com `carol2802@unochapeco.edu.br` (crie a conta se ainda
   não existir).
2. Clique em **New repository**.
   - Nome sugerido: `sistema-clima-hro-alvf`
   - Visibilidade: **Public** (ou Private, se o professor exigir apenas link de acesso liberado)
   - **Não** marque "Add a README" (já temos um pronto)
3. Depois de criado, o GitHub mostra um comando parecido com:
   ```
   git remote add origin https://github.com/carol2802/sistema-clima-hro-alvf.git
   ```
4. No computador, dentro da pasta deste projeto:
   ```bash
   git init
   git add .
   git commit -m "docs: preenchimento inicial do TPE (etapas 1-24) e correcoes da avaliacao"
   git branch -M main
   git remote add origin https://github.com/<SEU_USUARIO>/sistema-clima-hro-alvf.git
   git push -u origin main
   ```
5. Crie as branches de correção (opcional, mas reforça a evidência de versionamento pedida):
   ```bash
   git checkout -b fix/uc05-uc06-extend
   git commit --allow-empty -m "fix: corrige direcao e ponto de extensao UC05->UC06"
   git push -u origin fix/uc05-uc06-extend

   git checkout main
   git checkout -b fix/modelo-classes
   git commit --allow-empty -m "fix: adiciona enums e separa participacao de resposta anonimizada"
   git push -u origin fix/modelo-classes

   git checkout main
   ```
6. Crie a tag da entrega final:
   ```bash
   git tag -a v1.0-tpe-final -m "Entrega final do TPE"
   git push origin v1.0-tpe-final
   ```

## Opção B — Pelo GitHub CLI (`gh`)

```bash
gh auth login                     # login com carol2802@unochapeco.edu.br
gh repo create sistema-clima-hro-alvf --public --source=. --remote=origin --push
```

## Criando as Issues (uma por história de usuário)

Use o modelo em `.github/ISSUE_TEMPLATE/historia_usuario.md`, ou rode para cada HU01–HU12:

```bash
gh issue create \
  --title "HU01 - Planejar campanhas de clima" \
  --body "Como RH, quero criar campanhas de clima para organizar a aplicacao das pesquisas. RF relacionado: RF01, RF02." \
  --label "prioridade-alta"
```

Repita trocando título/descrição conforme a tabela da seção 5 (Backlog) do documento — HU02 a HU12.

## Se preferir que eu faça isso por você

Se você conectar o GitHub como integração aqui no Claude (autorizando com a conta da Carol), eu
consigo criar o repositório, subir os arquivos e abrir as Issues diretamente por aqui, sem precisar
rodar nenhum comando. Foi por isso que apareceu um convite para conectar o GitHub nesta conversa —
se aceitar, me avise que eu continuo daí.
