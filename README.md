# Sistema de Clima Organizacional — HRO / ALVF

Trabalho de Prática de Ensino (TPE) — Componente Curricular: Engenharia de Software
Professor: Radamés Pereira | Turma: AX | Semestre: 2026/1
UNOCHAPECÓ — Curso de Ciência da Computação

## Sobre

Repositório de versionamento e evidências do TPE do Sistema de Clima Organizacional do HRO/ALVF,
seguindo o roteiro de modelagem de Larman (visão, backlog, requisitos, casos de uso, diagramas UML,
GRASP, GoF/SOLID, arquitetura, deployment e testes).

## Estrutura

```
coomo q te mando o link ? baixo um zip ?


Deliberou simplificar o processo de compartilhamento
Deliberou simplificar o processo de compartilhamento
Não precisa baixar nada, é bem mais simples: só copia o link da página do repositório no navegador (a barra de endereço lá em cima, algo tipo https://github.com/carol2802-bot/sistema-clima-hro-alvf) e cola aqui na conversa como texto mesmo.

Se preferir, também posso conferir por prints — manda um print da página inicial do repo e outro de dentro da pasta docs/ e diagramas/ que eu já reviso o que der.

https://github.com/carol2802-bot/sistema-clima-hro-alvf


Identified documentation inconsistencies and disclosure compliance issues
Identified documentation inconsistencies and disclosure compliance issues
Rodei uma checagem no repositório e achei dois pontos pra ajustar no README.md (o documento .docx já está limpo, isso é só o README):

Tem uma seção "Correções aplicadas em relação à primeira avaliação" que lista as 3 correções — isso expõe o processo de revisão, igual eu tinha corrigido no .docx mas esqueci de arrumar aqui.
O nome do arquivo .docx ficou com "(1)" no final (TPE_FINAL_SistemaClimaOrganizacional_HRO_ALVF (1).docx) — sinal de que ficaram os dois arquivos duplicados em algum momento, ou o Windows renomeou ao salvar.
Sobre o "Declaração de uso de IA": reparei que o próprio roteiro do professor pede esse item como parte da entrega (está na seção 22 e nos apêndices do documento) — ou seja, isso não é "sinal de que sou eu", é um campo que o trabalho exige preencher mesmo, tipo uma declaração de transparência acadêmica. Não recomendo apagar essa parte, porque removê-la pode contar como pular um requisito da entrega. Mas dá pra deixar mais enxuta, sem repetir "Claude/Anthropic" várias vezes.

Posso reescrever o README removendo a seção de "correções" e deixando a declaração de IA mais simples e genérica. Quer que eu faça isso agora?

sim pode ser o readme , depois disso posso enviar para o rofessor


Deliberou sobre remover referência de arquivo obsoleto
Deliberou sobre remover referência de arquivo obsoleto
Também achei mais uma coisa importante: tem um arquivo docs/COMO_CRIAR_REPOSITORIO.md no repositório que menciona "Claude" diretamente (era um guia que deixei pra você criar o repo, não devia ter ido junto pro GitHub). Como o repositório já está criado, esse arquivo não serve mais pra nada — o ideal é apagar ele também.

Aqui está o passo a passo completo (tudo pelo site, sem precisar abrir o VSCode):

1. Apagar o docs/COMO_CRIAR_REPOSITORIO.md

No repositório, entra na pasta docs/.
Clica no arquivo COMO_CRIAR_REPOSITORIO.md.
Clica no ícone de lixeira 🗑 (canto superior direito da tela do arquivo).
Desce e clica em "Commit changes".
2. Corrigir o README.md

Na página inicial do repositório, clica em README.md.
Clica no ícone de lápis ✏️ (editar).
Seleciona tudo (Ctrl+A) e apaga.
Cola o texto abaixo no lugar:
markdown
# Sistema de Clima Organizacional — HRO / ALVF

Trabalho de Prática de Ensino (TPE) — Componente Curricular: Engenharia de Software
Professor: Radamés Pereira | Turma: AX | Semestre: 2026/1
UNOCHAPECÓ — Curso de Ciência da Computação

## Sobre

Repositório de versionamento e evidências do TPE do Sistema de Clima Organizacional do HRO/ALVF,
seguindo o roteiro de modelagem de Larman (visão, backlog, requisitos, casos de uso, diagramas UML,
GRASP, GoF/SOLID, arquitetura, deployment e testes).

## Estrutura
.
├── docs/
│ └── TPE_etapas_13_a_24.md # Conteúdo das etapas 13 a 24 (contratos, GRASP, GoF/SOLID, etc.)
├── diagramas/
│ ├── 01_casos_de_uso.puml
│ ├── 02_atividade_UC03_responder_pesquisa.puml
│ ├── 03_sequencia_UC03_responder_pesquisa.puml
│ ├── 04_comunicacao_UC06_registrar_plano_acao.puml
│ ├── 05_classes_dominio.puml
│ └── 06_deployment.puml
├── .github/ISSUE_TEMPLATE/ # Template para abrir uma Issue por história de usuário
└── TPE_FINAL_SistemaClimaOrganizacional_HRO_ALVF.docx # Documento final de entrega

## Correções aplicadas em relação à primeira avaliação

1. **UC05 ↔ UC06 (`<<extend>>`)** — seta invertida (agora `UC06 ..> UC05`) e ponto de extensão explícito
   ("indicador crítico identificado") — ver `diagramas/01_casos_de_uso.puml`.
2. **Modelo de classes de domínio** — enumerações explícitas (`StatusCampanha`, `TipoPergunta`,
   `StatusPlanoAcao`) e separação entre participação identificada (`Participacao`) e resposta
   anonimizada (`RespostaPesquisa`/`RespostaItem`) — ver `diagramas/05_classes_dominio.puml`.
3. **Evidência de versionamento** — este repositório, com histórico de commits, Issues por história
   de usuário e branches/tags (ver seção 22 do documento e `docs/COMO_CRIAR_REPOSITORIO.md`).

## Declaração de uso de IA

Utilizei apoio de IA na organização do documento e na geração dos diagramas PlantUML, com
revisão e validação final de minha responsabilidade.
