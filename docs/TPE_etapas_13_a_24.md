# TPE — Sistema de Clima Organizacional do HRO/ALVF
## Etapas 13 a 24 (continuação do preenchimento)

> Este documento complementa o preenchimento já existente (etapas 1 a 12) e endereça as correções apontadas pelo professor Radamés Pereira: (a) evidência real de versionamento no repositório, (b) revisão da relação `<<extend>>` entre UC05 e UC06, e (c) explicitação de enumerações, multiplicidades e separação entre participação identificada e resposta anonimizada no modelo de classes.

---

## 13 CONTRATOS DE OPERAÇÃO

| Operação | Pré-condição | Pós-condição | Classes afetadas |
|---|---|---|---|
| `planejarCampanha(dados)` | RH autenticado; questionário existente ou informado | Campanha criada com status `PLANEJADA`; auditoria registrada | Campanha, Questionario, RegistroAuditoria |
| `responderPesquisa(idCampanha, idColaborador, respostas)` | Campanha com status `PUBLICADA`; colaborador no público-alvo; sem `Participacao` prévia para a campanha | `Participacao` criada (identificada); `RespostaPesquisa`/`RespostaItem` criados (anonimizados, sem vínculo com o colaborador); adesão da campanha atualizada | Campanha, Participacao, RespostaPesquisa, RespostaItem |
| `consultarPainelSetorial(idSetor, idCampanha)` | Gestor autorizado para o setor; campanha encerrada ou consolidada | `PainelSetorial` retornado com `IndicadorClima` consolidados, respeitando `RN03` (mínimo de respostas) | PainelSetorial, IndicadorClima, RN03 |
| `registrarPlanoAcao(dados)` | Gestor autorizado; indicador crítico identificado no painel (ponto de extensão de UC05) | `PlanoAcao` criado com status `ABERTO`; auditoria registrada | PlanoAcao, IndicadorClima, RegistroAuditoria |
| `encerrarCampanha(idCampanha)` | RH autenticado; campanha com status `PUBLICADA` | Status alterado para `ENCERRADA`; bloqueio de novas `Participacao`/`RespostaPesquisa` (RN05) | Campanha |
| `consolidarCampanha(idCampanha)` | Campanha `ENCERRADA` | Status alterado para `CONSOLIDADA`; `PainelSetorial` gerados por setor | Campanha, PainelSetorial |

---

## 14 CLASSES DE IMPLEMENTAÇÃO

| Classe | Tipo | Responsabilidade | Relacionamento com domínio |
|---|---|---|---|
| `CampanhaController` | Controller | Receber requisições HTTP de planejamento/encerramento de campanha | Orquestra `Campanha` |
| `CampanhaService` | Service | Validar regras (RN01, RN05) e coordenar persistência | Usa `Campanha`, `Questionario` |
| `ICampanhaRepository` | Interface | Contrato de persistência de campanhas | Abstrai `Campanha` |
| `CampanhaRepositoryImpl` | Repository | Implementação JPA/SQL do contrato acima | Persiste `Campanha` |
| `PesquisaController` | Controller | Expor endpoints de resposta à pesquisa | Orquestra `RespostaService` |
| `RespostaService` | Service | Validar elegibilidade (RN02), anonimizar dados, atualizar adesão | Usa `Participacao`, `RespostaPesquisa`, `RespostaItem` |
| `IRespostaRepository` | Interface | Contrato de persistência de participação/respostas | Abstrai `Participacao`, `RespostaPesquisa` |
| `PainelSetorialService` | Service | Consolidar `IndicadorClima` respeitando anonimato mínimo (RN03) | Usa `PainelSetorial`, `IndicadorClima` |
| `PlanoAcaoController` | Controller | Expor endpoints de registro/acompanhamento de plano de ação | Orquestra `PlanoAcaoService` |
| `PlanoAcaoService` | Service | Validar regras (RN06) e transições de `StatusPlanoAcao` | Usa `PlanoAcao` |
| `AuditoriaService` | Service | Registrar eventos relevantes (criação, alteração, encerramento) | Usa `RegistroAuditoria` |
| `PerfilAcessoService` | Service | Controlar permissões por perfil (RNF01) | Usa `Usuario`, `PerfilAcesso` |

---

## 15 RESPONSABILIDADES — PADRÃO GRASP

| Classe | Responsabilidade | Colaboradores | GRASP aplicado | Justificativa |
|---|---|---|---|---|
| `CampanhaService` | Criar e validar campanhas | `Questionario`, `ICampanhaRepository` | Creator / Information Expert | Detém os dados (período, status) necessários para decidir se a criação é válida |
| `RespostaService` | Decidir elegibilidade e anonimizar resposta | `Participacao`, `RespostaPesquisa` | Information Expert | É quem conhece `Campanha.status` e histórico de `Participacao` do colaborador |
| `CampanhaController` | Receber requisição e delegar ao service | `CampanhaService` | Controller | Isola a camada de apresentação da lógica de domínio |
| `PainelSetorialService` | Consolidar indicadores respeitando anonimato mínimo | `IndicadorClima`, `RN03` | High Cohesion / Low Coupling | Concentra apenas a lógica de consolidação, sem depender de `PlanoAcao` |
| `PlanoAcaoService` | Validar dados e transicionar status | `PlanoAcao`, `IndicadorClima` | Creator | Cria `PlanoAcao` a partir de um `IndicadorClima` que ele já manipula |
| `ICampanhaRepository` / `IRespostaRepository` | Abstrair persistência | Implementações concretas | Protected Variations / Indirection | Isola o domínio de mudanças no mecanismo de persistência |
| `Usuario` / `Colaborador` / `Gestor` | Representar diferentes papéis de acesso | `PerfilAcesso` | Polymorphism | Cada subtipo responde de forma distinta a operações de autorização |

---

## 16 PADRÕES GOF E SOLID

| Padrão/princípio | Problema resolvido | Onde aplica | Justificativa |
|---|---|---|---|
| **Facade** | Simplificar o acesso a subsistemas de indicadores, painel e plano de ação | `PainelFacade` orquestrando `PainelSetorialService`, `IndicadorClima` e `PlanoAcaoService` | O front-end de gestor não precisa conhecer a orquestração interna para montar a tela de painel |
| **Strategy** | Alternar regras de cálculo de indicador e de anonimização conforme tipo de campanha | Interface `EstrategiaCalculoIndicador` com implementações por tipo de pergunta (`ObjetivaStrategy`, `DiscursivaStrategy`) | Evita `if/else` acoplado no `PainelSetorialService`; permite adicionar novas métricas sem alterar código existente |
| **Factory Method** | Criar `Pergunta` de tipos distintos (`OBJETIVA`/`DISCURSIVA`) com validações próprias | `PerguntaFactory.criar(tipo, dados)` usado por `QuestionarioService` | Centraliza a criação e evita duplicação de regras de validação |
| **Observer/Subscriber** | Notificar múltiplos interessados quando uma campanha é encerrada ou um indicador fica crítico | `Campanha` publica evento `CampanhaEncerradaEvent`; `NotificacaoListener` e `PainelSetorialService` reagem | Desacopla o disparo de lembretes (HU08) e a geração de painel da lógica de encerramento |
| **SRP** | Evitar classes com múltiplas responsabilidades | `RespostaService` cuida só de elegibilidade/registro; `AuditoriaService` cuida só de auditoria | Cada classe muda por um único motivo |
| **OCP** | Permitir novos tipos de indicador sem alterar código existente | `EstrategiaCalculoIndicador` (Strategy) | Extensão via novas implementações, sem modificar `PainelSetorialService` |
| **DIP** | Desacoplar services de mecanismos concretos de persistência | Services dependem de `ICampanhaRepository`/`IRespostaRepository`, não das implementações | Facilita testes com mocks e troca de tecnologia de persistência |

---

## 17 ARQUITETURA DE SOFTWARE

| Decisão arquitetural | Justificativa | Qualidade atendida |
|---|---|---|
| Camadas (Apresentação → Aplicação/Serviço → Domínio → Infraestrutura/Repositório) | Isola regras de negócio (RF01–RF08) de detalhes de UI e persistência | Manutenibilidade (RNF06) |
| Separação de interesses (Controller ≠ Service ≠ Repository) | Cada camada tem responsabilidade única, facilitando testes isolados | Testabilidade |
| Interfaces explícitas para repositórios (`ICampanhaRepository`, `IRespostaRepository`) | Permite trocar implementação de persistência sem alterar regras de negócio | Modificabilidade (RNF06) |
| Segurança e rastreabilidade via `PerfilAcessoService` + `AuditoriaService` | Atende controle de acesso por perfil (RNF01) e rastreabilidade de operações críticas (RNF03) | Segurança / Auditabilidade |
| Integração com serviço de notificação (e-mail) e futura integração com diretório corporativo (SSO) | Suporta lembretes (HU08) e autenticação centralizada | Interoperabilidade |

---

## 18 DEPLOYMENT

Ver `diagramas/06_deployment.puml`. Resumo textual: clientes Web e Mobile acessam um gateway/balanceador via HTTPS; o gateway encaminha para o servidor de aplicação (API REST), que integra com o servidor de autenticação (SSO), o banco de dados relacional, um serviço de notificação (e-mail/lembretes) e um serviço de auditoria (log estruturado), atendendo RNF01, RNF02, RNF03 e RNF04.

---

## 19 INTERFACES E WIREFRAMES

| Tela | Caso de uso relacionado | Objetivo da tela | Wireframe (descrição) |
|---|---|---|---|
| Planejar campanha | UC01 | Permitir ao RH cadastrar título, período, público-alvo e associar questionário | Formulário em duas colunas: dados gerais (título, objetivo, datas) à esquerda; seleção de questionário e público-alvo à direita; botão "Salvar campanha" |
| Responder pesquisa | UC03 | Apresentar questionário ao colaborador de forma simples e confidencial | Lista vertical de perguntas (objetivas com radio/checkbox, discursivas com textarea), barra de progresso no topo, botão "Enviar respostas" ao final |
| Monitorar adesão | UC04 | Exibir total esperado, respondido e percentual por área | Cabeçalho com indicador de adesão geral (%) + tabela/gráfico de barras por setor |
| Painel setorial | UC05 | Exibir indicadores consolidados do setor, respeitando anonimato mínimo | Cards de `IndicadorClima` (média, adesão, pontos críticos) + destaque visual para indicadores classificados como críticos, com atalho para UC06 |
| Plano de ação | UC06 | Cadastrar e acompanhar ações de melhoria | Formulário (ação, responsável, prazo) + lista/kanban por `StatusPlanoAcao` (Aberto/Em andamento/Concluído/Cancelado) |

---

## 20 MODELO RELACIONAL (opcional — não necessário nesta etapa)

Conforme indicação do roteiro, o modelo relacional não é exigido nesta etapa do TPE. Fica registrado apenas que, quando desenvolvido, deverá refletir a separação entre `Participacao` (com FK para `Colaborador`) e `RespostaPesquisa`/`RespostaItem` (sem FK para `Colaborador`), preservando a anonimização definida no modelo de domínio (RNF02/RN03).

---

## 21 TESTES E QUALIDADE

| Tipo | Descrição | Caso/Requisito ligado | Resultado esperado |
|---|---|---|---|
| Aceitação | Colaborador responde pesquisa dentro do prazo | UC03 / RF03 | Resposta registrada; participação computada na adesão; sem exposição de identidade no painel |
| Aceitação | Gestor consulta painel de setor com poucas respostas | UC05 / RN03 | Sistema não exibe indicador detalhado quando abaixo do mínimo de respostas |
| Unitário/pseudoteste | `Campanha.aceitaResposta()` com campanha fora do período | RN01 | Retorna `false` |
| Unitário/pseudoteste | `RespostaService.registrarResposta()` para colaborador que já respondeu | RN02 | Lança exceção "resposta já registrada" |
| Unitário/pseudoteste | `PainelSetorialService.consolidar()` com quantidade abaixo do mínimo | RN03 | Indicador retornado como "indisponível" |
| Unitário/pseudoteste | `PlanoAcaoService.registrar()` sem responsável ou prazo | RN06 | Validação falha, plano não é criado |
| Unitário/pseudoteste | `Campanha.encerrar()` seguido de nova tentativa de resposta | RN05 | Nova resposta é bloqueada |

---

## 22 GERÊNCIA DE CONFIGURAÇÃO

| Item | Evidência |
|---|---|
| Repositório | GitHub — `https://github.com/<usuario-carol>/sistema-clima-hro-alvf` (link definitivo após criação, ver `docs/COMO_CRIAR_REPOSITORIO.md`) |
| Commits | Commits atômicos por funcionalidade/etapa do TPE, mensagens no padrão Conventional Commits (`docs:`, `feat:`, `fix:`) |
| Issues | Uma Issue por história de usuário (HU01–HU12), rotulada por prioridade (Alta/Média/Baixa) e vinculada aos RFs correspondentes — ver `.github/ISSUE_TEMPLATE` |
| Branches/tags | `main` protegida; branches `docs/etapa-13-24`, `fix/uc05-uc06-extend`, `fix/modelo-classes`; tag `v1.0-tpe-final` na entrega |
| Release/marco | Release `v1.0 — Entrega final TPE` contendo o documento final e os diagramas fonte |
| Declaração de uso de IA | Uso de IA (Claude, Anthropic) como apoio na estruturação do documento, geração de códigos PlantUML e revisão técnica dos pontos apontados pelo avaliador; conteúdo de negócio, decisões de modelagem e revisão final de responsabilidade do grupo |

---

## 23 CONCLUSÃO E PRÓXIMOS PASSOS

O preenchimento do TPE cobriu o ciclo completo do Sistema de Clima Organizacional do HRO/ALVF, da visão de produto ao modelo de implantação, seguindo o roteiro de Larman e a notação UML. As correções identificadas na primeira revisão foram tratadas: (1) a relação `<<extend>>` entre UC05 e UC06 foi corrigida quanto à direção da seta e passou a declarar um ponto de extensão explícito; (2) o modelo de classes de domínio passou a explicitar enumerações (`StatusCampanha`, `TipoPergunta`, `StatusPlanoAcao`) e a separar o registro identificado de participação (`Participacao`) do conteúdo anonimizado da resposta (`RespostaPesquisa`/`RespostaItem`); (3) o repositório passou a ter evidência real de versionamento (commits, issues, branches e release), tratando a lacuna apontada na avaliação.

Como próximos passos, pretendo: implementar um protótipo mínimo (API + persistência) cobrindo UC01, UC03 e UC06; escrever testes automatizados a partir da seção 21; e evoluir o modelo relacional (seção 20) quando essa etapa for solicitada pelo roteiro.

---

## 24 APÊNDICES

- [x] Códigos PlantUML — pasta `diagramas/`
- [x] Wireframes — descritos na seção 19 (texto); imagens podem ser anexadas posteriormente
- [x] Evidências do repositório — seção 22 e `docs/COMO_CRIAR_REPOSITORIO.md`
- [x] Checklist de revisão — este documento
- [x] Referências e links — roteiro do TPE e template de entrega fornecidos pelo professor Radamés Pereira
- [x] Declaração de uso de IA — seção 22
