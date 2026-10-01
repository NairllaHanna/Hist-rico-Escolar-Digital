# Histórico Escolar Digital (SED) — Regras Consolidadas

Documento-fonte para reproduzir o protótipo em outra ferramenta/IA. Consolida PRD v2/v2.1 + todas as decisões tomadas durante a construção. Em caso de conflito, **este documento prevalece**.

---

## 1. Perfis e responsabilidades

| Perfil | Pode | Não pode |
|---|---|---|
| **AOE** (Agente de Organização Escolar) | Iniciar histórico não elaborado; continuar histórico **Em elaboração**; preencher Estudos, Notas, Observações, Revisão; **Salvar** e **Voltar** | Assinar; **Enviar para aprovação**; Retificar; Liberar Emissão; Tornar Sem Efeito. Nunca consta como assinante. |
| **GOE** (Gerente de Organização Escolar) | Elaborar, assinar (GOV.BR) e **enviar para aprovação**; retificar históricos emitidos/reprovados | — |
| **Diretor** | Aprovar/Devolver (individual e em lote); **Aprovar retificação** | — |
| **Supervisor de Ensino** | Validar/Devolver (individual e em lote); **Validar retificação**; **Tornar Sem Efeito** | — |
| **URE / Dirigente** | Consulta/relatórios na própria URE | Ações de fluxo |
| **Órgão Central** (SUPLAN‑DIMAV‑COVESC) | **Acesso total**: todas as ações de todos os perfis + exclusivas (Log de Auditoria, Liberar Emissão, Excluir disciplina, Textos Legais, Parâmetros), visão estadual | — |

Regras gerais:
- Assinaturas digitais válidas: **somente GOE, Diretor e Supervisor**.
- Toda ação é **parametrizável por perfil** (tela *Parâmetro do Histórico Escolar*). Ação não permitida para o perfil/status → **não exibir/desabilitar** (ícone cinza), em vez de permitir clique e mostrar erro.
- Escopo hierárquico: Escola vê só a própria escola; URE vê a URE; Órgão Central vê tudo (grid, cards e relatórios).

---

## 2. Status e fluxo

### 2.1 Fluxo normal (unidade **com** GOE alocado)
`Não Emitido → Em elaboração → (GOE envia) Aguardando Aprovação → (Diretor aprova) Aguardando Validação → (Supervisor valida) Emitido`

- Devolução pelo Diretor ou Supervisor → **Devolvido** (justificativa obrigatória). GOE corrige e reenvia → volta para **Aguardando Aprovação** e passa novamente pelo Diretor.
- O motivo da devolução aparece em banner destacado quando o GOE reabre o documento.

### 2.2 Fluxo excepcional (unidade **sem** GOE alocado)
- O sistema **verifica na SED se a unidade possui GOE alocado** (no protótipo: opção "Unidade sem GOE" ao lado do perfil Diretor).
- **Somente** quando não houver GOE: Diretor tem acesso ao início do fluxo (Emitir, editar Estudos/Notas, enviar).
- Ao enviar, o Diretor gera **duas assinaturas** (1ª emissão/elaboração, 2ª aprovação) e o documento vai **direto para Aguardando Validação**, sem passar por Aguardando Aprovação (o Diretor não aprova algo que ele mesmo enviou).
- Com GOE alocado, a dupla assinatura do Diretor **não** se aplica.

### 2.3 Retificação (documento Emitido ou Retificação Reprovada)
`Emitido → (GOE envia) Aprovar Retificação → (Diretor aprova) Validar Retificação → (Supervisor valida) Emitido`
- Reprovação em qualquer etapa → **Retificação Reprovada** (justificativa obrigatória; GOE vê banner vermelho com quem/quando/por quê).
- Alterações ficam **pendentes** (`retPendente`) e só são aplicadas após validação do Supervisor; se reprovadas, são descartadas. **O documento publicado anterior permanece vigente** enquanto em análise.
- Mantém o **mesmo número de publicação**; versão anterior preservada para auditoria.
- **Unidade sem GOE alocado** (sistema verifica na SED): o **Diretor** inicia, preenche e envia a retificação; ao enviar registra as **duas responsabilidades/assinaturas** (solicitação + aprovação) e o documento vai **direto para Aguardando Validação da Retificação** → Supervisor valida/reprova → Emitido ou Retificação Reprovada. Com GOE alocado, o fluxo normal se mantém (Diretor não retifica).
- Dois tipos:
  - **Nota**: múltiplas notas por solicitação (Componente + Ano + Nova nota). Justificativa + anexo obrigatórios. Nova nota ≠ nota atual; não repetir Componente+Ano na mesma solicitação. Faixa **5–10** ou N/A, ET, EP, ES.
  - **Dado pessoal**: sem justificativa (correção feita na Ficha do Aluno); gera diff automático.

### 2.4 Tornar Sem Efeito (Supervisor)
- Só para **Emitido**. Justificativa (dropdown) + anexo obrigatórios.
- Opção de justificativa: "Parecer técnico do **Chefe do SEGRE/SEVESC**" (nomenclatura antiga "Diretor do CIE/NVE" foi substituída).
- Registro com destaque no Log; imutável e **irreversível**. A versão invalidada permanece definitivamente **Sem Efeito**, com histórico e publicação preservados para consulta/auditoria.
- O **número de publicação anterior nunca é reutilizado**.
- Nova emissão = **novo ciclo completo** (elaboração → assinatura → aprovação → validação); após a validação final é gerado **novo número de publicação**, com **vínculo histórico** à publicação Sem Efeito.

### 2.5 Liberar Emissão (Órgão Central / perfil autorizado)
- Aluno **sem rendimento aprovado**: o fluxo **não pode ser iniciado** — ao clicar em Emitir: alerta "Aluno não possui rendimento aprovado." (não existe mais permissão "enviar sem rendimento").
- Ação **Liberar Emissão**: modal com justificativa (Regularização de Vida Escolar / Convalidação / Pedido Judicial) + anexo obrigatório. Após liberar, a escola pode emitir. Registrado em auditoria.

---

## 3. Elaboração — abas

### 3.1 Estudos Realizados
- Percurso derivado do **ano de conclusão** do aluno (não fixo).
- **Editar** (lápis) libera a linha; pode editar quantas vezes quiser. Alterações exigem **justificativa + anexo** e são registradas em "Informações modificadas".
- **O ano de um estudo não pode ser alterado** na edição; para mudar, excluir e cadastrar novo.
- **Adicionar Estudos Realizados** (modal): UF, Município, Ano, Série, Rede, Estabelecimento, Total de aulas BNCC, Total de aulas Parte Diversificada, **Total de Carga Horária** (campo numérico digitado; **não** é soma dos outros dois, pois aqueles são total de aulas). Os três totais ficam na mesma linha. Campos numéricos são texto numérico, não dropdown.
  - UF = SÃO PAULO → Estabelecimento é **dropdown**; outro estado → **texto livre**.
  - Trocar UF limpa Município e Estabelecimento; trocar Município/Rede limpa Estabelecimento.
  - Justificativa + anexo obrigatórios (inclui "Reclassificação indevida (Convalidação)").
  - Botão aparece quando o percurso está incompleto (ex.: aluno de outro estado/rede sem dados na SED).
- **Adicionar um estudo cria automaticamente a coluna do ano/série na aba Notas.**
- **Excluir um estudo** pede confirmação: "Ao excluir este Estudo Realizado, todas as disciplinas e notas vinculadas a este ano também serão excluídas. Deseja continuar?" (Excluir / Cancelar) e remove a coluna correspondente em Notas.
- **Aluno reclassificado**: aviso azul fino "Aluno reclassificado — O aluno foi reclassificado do X para o Y em AAAA. Caso esteja incorreta, *adicione os estudos realizados* (link). Caso esteja correta, siga com a emissão." Nesse caso **não** exibir o botão "Adicionar estudo realizado" (inserção via link).

### 3.2 Notas
- **Todas as notas de todos os anos/séries devem estar preenchidas para enviar** à próxima etapa. Campos pendentes ficam destacados (borda vermelha) + mensagem.
- Rede Estadual de SP: aceitar **somente inteiros 5 a 10** ou **N/A, ET, EP, ES**. Sem decimais. "-" não é válido.
- **Estudo de outro estado, rede municipal ou outro sistema**: campo **também obrigatório**, porém **sem restrição de valor** (registrar conforme documento de origem).
- **Envio bloqueado sem Texto Legal aplicável** (Tipo de Ensino + Ano de Conclusão + vigência): "Não foi localizado texto legal vigente para o tipo de ensino e ano de conclusão. Solicite a parametrização ao Órgão Central." O documento permanece no status atual (pode salvar), sem assinatura e sem avançar. Vale também para o Diretor em unidade sem GOE.
- Disciplina adicionada fica vinculada ao **ano/série** escolhido; anos não aplicáveis aparecem como "—" e não são obrigatórios.
- **Justificativa de alteração de nota**:
  - **Não** pedir durante a confecção (lançar, salvar, avançar e voltar sem ter enviado).
  - **Pedir** (justificativa + anexo) ao alterar nota que já veio cadastrada (ex.: migrada) **ou** em documento **Devolvido** (notas existentes no momento da devolução).
- Campos alterados manualmente (nota/BNCC/PD/CH/observações) ficam **destacados em amarelo** no documento interno.

### 3.3 Adicionar disciplina (modal)
- Texto: "Preencha os campos abaixo para incluir uma disciplina."
- Campos: **Ano/Série**, Área de conhecimento, Componente curricular, **Classificação** (preenchida automaticamente conforme disciplina — Base Comum ou Parte Diversificada; caixa bloqueada).
- **Justificativa + anexo obrigatórios.**
- **Sem duplicidade:** o dropdown Componente Curricular lista apenas disciplinas ainda não vinculadas ao ano/série escolhido; validação de integridade também bloqueia duplicidade no salvamento.

### 3.4 Excluir disciplina (Órgão Central)
- Sem nota/conceito: "Tem certeza de que deseja excluir a disciplina [nome]?" — **Sim, excluir** / **Cancelar**.
- Com nota/conceito: "Ao excluir esta disciplina, a nota/conceito registrado também será excluído. Deseja continuar?" — **Excluir** / **Cancelar**.
- Ao confirmar: remove disciplina e notas, atualiza a grid e registra no Log os valores removidos. Cancelar não altera nada.

### 3.5 Modais com informação não salva (disciplina e estudo)
- Ao fechar por **X, Cancelar/Voltar, clique fora** ou qualquer ação que descarte dados: "Existem informações preenchidas que ainda não foram salvas. Deseja realmente fechar esta tela?" — **Continuar editando** / **Fechar sem salvar**.
- Sem alteração → fecha direto.

### 3.6 Observações e Revisão
- Observações: sem justificativa/anexo.
- Revisão (AOE): Salvar e Voltar, **sem** "Enviar para aprovação"; status permanece Em elaboração; outro perfil continua o mesmo documento (sem cópia).

---

## 4. Aprovação / Validação
- Diretor e Supervisor: **não** há campo "Observação"; existe **Justificativa**, que só aparece se a ação for **Devolver** (título do modal = "Devolver"; em retificação = "Reprovar").
- **Aprovar/Validar em lote**: só aparece no filtro **Turma** para status Aguardando Aprovação / Aguardando Validação. Checkbox "Todos" abaixo do título "Selecionar". Gera **um registro de auditoria por aluno**.
- Todas as datas usam data/hora real do sistema (servidor).

---

## 5. Documento (PDF / Consulta)
- **Mesma tela de consulta para todos os perfis**, somente visualização; reflete a versão atual e as assinaturas efetivamente realizadas.
- **Consultar = Baixar (grid) = Exportar (consulta)** — sempre o mesmo arquivo/versão.
- Não emitido → marca d'água "documento não emitido/provisório". Emitido → versão definitiva com assinaturas GOV.BR.
- Cabeçalho e tipo de ensino refletem Fundamental/Médio; dados pessoais (nome/RG/CIN) vêm do **snapshot publicado**, nunca de dados pendentes.
- Cabeçalho de anos (Ano/Período Letivo, Anos Iniciais, Anos Finais) dimensionado pelos anos reais do aluno; grade contínua sem linhas quebradas.
- **QR Code** sempre presente no rodapé (aponta para Consulta Pública com o nº de publicação).
- Rodapé "Escala de Avaliação" vem dos **Textos Legais** (seção 8).

---

## 6. Consulta Pública de Documentos
- Busca por **RG** (número + dígito + UF), **CIN** ou **RA** (número 12 dígitos + **dígito** + **UF**, igual ao RG).
- **Tipo de Ensino = dropdown** (Ensino Fundamental de 9 anos / Ensino Médio).
- Data de nascimento + código da imagem (captcha).

---

## 7. RA (todas as telas de pesquisa)
- RA = 12 dígitos + dígito verificador (aceita X maiúsculo/minúsculo) + UF, em campos separados.
- Digitado/colado com menos de 12 dígitos → **completar com zeros à esquerda** (máscara aplicada no próprio campo ao sair). Com 12 dígitos → não alterar.

---

## 8. Parâmetro — Textos Legais do Histórico (Órgão Central)
- Cadastro: **Tipo de ensino** (Todos / Fundamental / Médio), **Vigência de** (ano), **Até** (ano) ou "Vigente até o momento", **Texto** (ex.: "Escala de Avaliação: A partir de 2007 - Escala numérica de notas de 0 a 10... Resolução SE 98/2008 e Deliberação CEE/SP nº 73/2008."). Pré‑visualização no modal.
- O histórico usa o texto cuja vigência contém o **ano de conclusão** e o tipo de ensino do aluno.
- Para cada **Tipo de Ensino + Ano de Conclusão** existe **no máximo um** texto aplicável. **"Todos os tipos de ensino" conflita com qualquer tipo específico** no mesmo período (nos dois sentidos). Fundamental e Médio entre si podem coincidir.
- Validação no cadastro e na edição. Mensagem: "Já existe um texto legal aplicável a este Tipo de Ensino no período de vigência informado. Ajuste a vigência para continuar." **Não ajustar/encerrar automaticamente** outros registros.
- **Situação calculada automaticamente** (não editável): Vigente se o período está em vigor; Encerrada se o ano final é anterior ao ano atual.
- Edição vale para históricos **ainda não emitidos**; emitidos preservam o texto usado na emissão (sem efeito retroativo).
- Exclusão: "Tem certeza de que deseja excluir este texto legal?" — Sim, excluir / Cancelar.
- Cadastro/edição/exclusão registrados no Log.

---

## 9. Relatório — Histórico Escolar
- Tipos: Emitidos, Retificados, Tornados sem efeito, Publicações vinculadas, Ações por usuário, Assinaturas GOV.BR.
- Filtros: Emissão de/até, **URE antes de Escola**, Escola, Tipo de Ensino, Turma. Limpar/Pesquisar; colunas ordenáveis.
- Cards‑resumo (Emitidos/Retificados/Sem Efeito/Pendentes) respeitam o escopo hierárquico.
- Exportar respeita filtros (mock no protótipo).

---

## 10. Log de Auditoria (Órgão Central; parametrizável para outros perfis)
- Menu entre "Relatório" e "Consulta Pública". **Somente leitura e imutável.**
- Filtros: período, RA (número + dígito + UF), nome, nº publicação, **URE, Escola**, tipo de ensino, status, tipo de ação, usuário, perfil.
- Grid: Data/Hora (DD/MM/AAAA HH:mm:ss), RA, Aluno, Escola, URE, Publicação, Ação (ícone + cor, não só cor), Usuário, Perfil, Status anterior, Status atual, Ver detalhes. Ordenação (padrão mais recente), paginação 10/25/50/100.
- Detalhe: identificação do aluno e documento, usuário/perfil/unidade no momento da ação, assinatura GOV.BR, justificativa, anexos, tabela **Campo / Anterior / Novo** (todas as alterações do mesmo salvamento num único evento), botão "Ver histórico completo" (linha do tempo).
- Cada ação gera registro (inclusive cada aluno de ação em lote); "Tornar sem efeito" com destaque.

---

## 11. Anexos e justificativas
- Todos os pontos com anexo aceitam **múltiplos arquivos** (cartão de prévia por arquivo, remover individual; "Nenhum anexo inserido" quando vazio). Validação exige ≥ 1.
- Justificativas são dropdowns (texto padronizado), exceto devolução/reprovação (texto livre).

---

## 12. Padrão visual (refinado pelo UX — Figma)
- Fonte Arial; texto 14px; títulos de modal 14px bold branco; rótulos 14–16px bold `#333/#3a3a47`.
- Azul primário `#459ad6`; verde confirmar `#0a802c`; vermelho `#dc2828`; cinza ícone inativo `#747486`; borda campo `#dddddd` / `#b6b6c0`.
- Campos 34px, raio 4px. Botões 34px, 14px; primário azul, secundário branco borda azul (Cancelar/Fechar ~148px).
- **Ícones de ação na grid: sem borda/círculo**, 22px, azul quando ativo, `#747486` quando indisponível (inclui Tornar Sem Efeito em azul).
- Grids estilo planilha: cabeçalho `#459ad6` 12px bold com divisórias `#5aa6dc`; células 14px com bordas `#cfdde9`; linhas alternadas `#eef5fc` / `#d9ebfb`; células sem quebra de linha (RA nunca quebra).
- Modais: cabeçalho azul 56px, raio 16px, corpo com bloco azul‑claro `#e9f6ff` raio 8px para "Dados do estudante/documento".
- Avisos: info azul fino (`#ebf7ff`, borda 0.5px `#459ad6`); alerta amarelo; sucesso verde; confirmações Sim/Não.
- Botões Voltar/Avançar com ícones de seta; sem link "Voltar para a pesquisa" (voltar pelo menu).

---

## 13. Dados de teste do protótipo
- 5.039 escolas (SED), 5.250 alunos em 30 escolas reais de SP.
- Casos: **ANA OLIVIA** (RA 000287463217‑5, reclassificada), **THIAGO HENRIQUE** (000287499201‑3, lacuna no percurso / outra rede), **LUARA** (000287509847‑1, sem rendimento aprovado).

---

## 14. Matriz Perfil × Status × Ação

Legenda: **D** = disponível · **I** = indisponível/inibida (ícone cinza na grid) · **–** = oculta (coluna/botão não exibido para o perfil).
Consultar e Baixar: **D para todos os perfis em todos os status** (mesmo documento; não emitido sai com marca de provisório).
"Editar/Salvar" referem-se às abas de elaboração (Estudos, Notas, Observações, Revisão).

### AOE
| Status | Emitir/Iniciar | Editar | Salvar | Enviar | Retificar | Demais ações de fluxo |
|---|---|---|---|---|---|---|
| Não Emitido | D¹ | D | D | – | – | – |
| Em elaboração | D | D | D | – | – | – |
| Devolvido | D | D | D | – | – | – |
| Demais status | I | I | I | – | – | – |

### GOE (e Diretor em unidade **sem GOE**, que assume estas ações)
| Status | Emitir/Iniciar | Editar | Salvar | Enviar | Retificar |
|---|---|---|---|---|---|
| Não Emitido | D¹ | D | D | D² | I |
| Em elaboração | D | D | D | D² | I |
| Devolvido | D | D | D | D² | I |
| Aguardando Aprovação / Validação | I | I | I | I | I |
| Emitido | I | I | I | I | D |
| Aguard. Aprovação/Validação da Retificação | I | I | I | I | I |
| Retificação Reprovada | I | I | I | I | D |
| Sem Efeito | D³ | I | I | I | I |

### Diretor
| Status | Aprovar | Devolver | Aprovar Retificação | Reprovar Retificação | Retificar |
|---|---|---|---|---|---|
| Aguardando Aprovação | D | D | I | I | I |
| Aguardando Aprovação da Retificação | I | I | D | D | I |
| Emitido / Retificação Reprovada | I | I | I | I | D só sem GOE⁴ |
| Demais status | I | I | I | I | I |

### Supervisor de Ensino
| Status | Validar | Devolver | Validar Retificação | Reprovar Retificação | Tornar Sem Efeito |
|---|---|---|---|---|---|
| Aguardando Validação | D | D | I | I | I |
| Aguardando Validação da Retificação | I | I | D | D | I |
| Emitido | I | I | I | I | D |
| Demais status | I | I | I | I | I |

### Órgão Central (SUPLAN‑DIMAV‑COVESC)
**Acesso total.** O Órgão Central pode executar **todas as ações de todos os perfis** em qualquer status em que a ação seja válida (Emitir, Editar, Salvar, Enviar, Aprovar, Devolver, Validar, Retificar, Aprovar/Validar Retificação, Tornar Sem Efeito), além das ações exclusivas:

| Ação exclusiva | Regra |
|---|---|
| Liberar Emissão | D quando o aluno **não possui rendimento aprovado**; I caso contrário |
| Excluir disciplina | D nas abas de elaboração (com confirmação) |
| Log de Auditoria / Textos Legais / Parâmetros | D |
| Relatórios | D com escopo estadual |

As validações de negócio continuam valendo para o Órgão Central (notas obrigatórias, CH, rendimento aprovado, Texto Legal aplicável, justificativas e anexos). Toda ação do Órgão Central é registrada no Log com o perfil utilizado.

Notas:
1. Emitir só é possível se o aluno tiver **rendimento aprovado**; senão, alerta "Aluno não possui rendimento aprovado." até a Liberação pelo Órgão Central.
2. Enviar exige: todas as notas preenchidas/válidas, CH preenchida, rendimento aprovado e **Texto Legal aplicável**. Diretor sem GOE: envio registra duas assinaturas e vai direto para Aguardando Validação.
3. A partir de Sem Efeito, Emitir inicia **novo ciclo** de elaboração (nova publicação ao final, vinculada à anterior).
4. Diretor sem GOE: envio da retificação registra duas assinaturas e vai direto para Aguardando Validação da Retificação (não há etapa de Aprovar Retificação).
