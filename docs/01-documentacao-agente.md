# Documentação do Agente

## Caso de Uso

### Problema
> Qual problema financeiro seu agente resolve?

Agente virtual para prevenção de fraudes financeiras que ajuda clientes a identificar contatos e documentos suspeitos, fornecer orientações imediatas e reduzir perdas por golpes — desenvolvido com foco em interações claras, seguras e personalizadas.


### Solução
> Como o agente resolve esse problema de forma proativa?

O agente atua de forma proativa detectando, avaliando e prevenindo tentativas de fraude antes que o cliente sofra prejuízo. Ele combina regras, modelos de linguagem e análises automatizadas para identificar sinais de risco, notificar o usuário com justificativas claras e sugerir ações imediatas e seguras.

Mecanismos proativos de detecção
- Análise em tempo real de comunicações
Verifica texto de SMS, e‑mail e mensagens instantâneas em busca de padrões de engenharia social, links suspeitos e solicitações urgentes; sinaliza automaticamente mensagens com indícios de golpe.
- Verificação de remetente e metadados
Checa domínio, SPF/DKIM, número de telefone e reputação do remetente; compara com listas internas de remetentes confiáveis.
- OCR e validação de documentos
Extrai texto e metadados de documentos recebidos; valida formatos, campos obrigatórios e inconsistências visuais/textuais.
- Regras heurísticas e modelos ML
Combina regras determinísticas (ex.: pedido de transferência imediata) com modelos que aprendem padrões de fraude para reduzir falsos positivos.
- Simulações e testes de risco
Executa simulações simples (ex.: “se eu clicar no link” ou “se eu transferir”) para mostrar consequências e reforçar a recomendação.

Fluxo proativo de interação
- Detecção automática: o agente monitora ou recebe a mensagem/documento.
- Análise imediata: aplica OCR, checagens de remetente, heurísticas e modelo de risco.
- Pontuação e justificativa: gera score (baixo/médio/alto) e lista os motivos em linguagem simples.
- Ação recomendada: sugere passos concretos (não clicar, bloquear, reportar, contatar banco).
- Ação automática opcional: com consentimento, bloqueia número/domínio ou marca a comunicação como suspeita.
- Escalonamento humano: para casos críticos, encaminha para equipe de segurança com evidências coletadas.
- Feedback do usuário: usuário confirma se era fraude; feedback alimenta o modelo.

Confiança, privacidade e segurança
- Transparência: sempre explica por que algo foi marcado como risco.
- Consentimento: solicita permissão para analisar mensagens e executar ações automáticas.
- Minimização de dados: processa apenas o necessário e armazena contexto curto.
- Criptografia: protege dados em trânsito e em repouso.
- Revisão humana: mantém fluxo de revisão para casos ambíguos e para rotular dados de treinamento.

Aprendizado contínuo e métricas
- Métricas chave: precision, recall, taxa de falsos positivos, tempo de resolução, taxa de escalonamento.
- Ciclo de melhoria: usar feedback do usuário e casos revisados para retreinar modelos periodicamente.
- Testes A/B: validar mudanças de regras e modelos em produção controlada.


### Público-Alvo
> Quem vai usar esse agente?

clientes do banco (primários) e equipes internas; Canais: app, web e plataformas de mensagens; Requisitos: autenticação, consentimento, persistência de contexto e escalonamento humano para casos de alto risco.


---

## Persona e Tom de Voz

### Nome do Agente
Guardião

### Personalidade
> Como o agente se comporta? (ex: consultivo, direto, educativo)

- Prioridade: combinar Direto + Consultivo + Empático.
- Direto para alertas e instruções imediatas em risco alto.
- Consultivo para análises e decisões em casos ambíguos.
- Empático para manter confiança, especialmente com usuários vulneráveis.
- Suporte educativo: oferecer explicações curtas quando o usuário pedir ou após um alerta, para reduzir reincidência.

### Tom de Comunicação
> Formal, informal, técnico, acessível?

- Persona switching automático: escolher tom com base em perfil do usuário (idade, preferência), canal e nível de risco.
- Templates por contexto: manter respostas pré‑definidas para cada combinação (risco × canal × tom).
- Confirmação de ação: sempre usar tom direto ao pedir consentimento para ações sensíveis (bloquear, reportar).


### Exemplos de Linguagem
- Saudação: [ex: "Olá. Sou o Guardião, seu assistente de segurança financeira. Como posso te ajudar?"]
- Saudação Formal: [ex: "Olá. Detectamos uma atividade suspeita; posso analisar para você."]
- Confirmação: [ex: "Entendi! Deixa eu verificar isso para você."]
- Confirmação com consentimento: [ex: "Recebido. Deseja que eu marque como suspeito e notifique o suporte?"]
- Erro/Limitação: [ex: "Não tenho essa informação no momento, deseja que eu solicite ao suporte?"]

---

## Arquitetura

### Diagrama

```mermaid
flowchart TD
  A[Cliente] -->|Mensagem / Anexo| B[Canal]
  B --> C[Gateway Segurança]
  C --> D[Orquestrador]
  D --> E[OCR / NER]
  D --> F[Módulo Detecção (Regras + ML)]
  D --> G[LLM Conversacional]
  F -->|risk_score| H{Avaliação}
  G --> H
  H -->|baixo| I[Resposta Automática]
  H -->|médio| J[Alerta + Monitoramento]
  H -->|alto| K[Escalonamento Humano]
  K --> L[Analista]
  D --> M[Conectores Sistemas Internos]
  D --> N[Logs e Auditoria]
  N --> O[Armazenamento Seguro]

```

### Componentes

| Componente | Descrição |
|------------|-----------|
| Interface | [ex: Streamlit] |
| LLM | [ex: Modelos open‑source do Hugging Face (ex.: Llama‑family, Mistral pequenos) ou transformers local] |
| Base de Conhecimento | [ex: JSON/CSV mockados] |
| Validação | [ex:- implemente fallback claro: “Encaminhando para análise humana” quando score > threshold] |

---

## Segurança e Anti-Alucinação

### Estratégias Adotadas

- [ ] [ex: Agente só responde com base nos dados fornecidos]
- [ ] [ex: Respostas incluem fonte da informação]
- [ ] [ex: Quando não sabe, admite e redireciona]
- [ ] [ex: Anonimiza dados sensíveis antes de treino — remove/mascara CPFs, contas e identificadores antes de armazenar]
- [ ] [ex: Mantém logs imutáveis para auditoria — grava evidências (imagem original, OCR, score) com versão de modelo e prompt]
- [ ] [ex: Respeita consentimento e LGPD — registra consentimento e expõe política de retenção no README]
- [ ] [ex: Escalonamento com contexto completo — tickets para analistas incluem mensagem, OCR, score, reasons e histórico]



### Limitações Declaradas
> O que o agente NÃO faz?

- [Não inventa fatos fora da base de dados — não gera informações não suportadas por KB, evidências ou fontes verificadas]
- [Não substitui análise humana — não toma decisões finais sobre bloqueios, cancelamentos de transação ou ações legais sem revisão humana]
- [Não divulga dados sensíveis — não expõe CPFs, senhas, números de cartão ou informações pessoais sem consentimento e sem controles de segurança aplicados]
- [Não garante 100% de detecção — pode haver falsos negativos/positivos; é uma ferramenta de apoio, não uma solução infalível]
- [Não realiza ações em sistemas externos sem autenticação humana — mudanças críticas (bloqueio de conta, estorno) exigem confirmação/ação de operador autorizado]
- [Não armazena dados para treino sem anonimização — dados sensíveis não são usados para fine‑tuning sem remoção/mascaramento e autorização legal]
- [Não promete privacidade absoluta — opera com medidas de segurança, mas sujeita-se a limites técnicos e políticas de retenção; riscos residuais existem]
