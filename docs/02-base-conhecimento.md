# Base de Conhecimento

## Dados Utilizados

Descreva se usou os arquivos da pasta `data`, por exemplo:

| Arquivo | Formato | Utilização no Agente |
|---------|---------|---------------------|
| `historico_atendimento.csv` | CSV | Contextualizar interações anteriores |
| `perfil_investidor.json` | JSON | Personalizar recomendações |
| `produtos_financeiros.json` | JSON | Sugerir produtos adequados ao perfil |
| `transacoes.csv` | CSV | Analisar padrão de gastos do cliente |
|`https://huggingface.co/api/resolvecache/models/TFOA2/automatic_fraude_detection/05c9735b5a8c9ff4b206957e5c4224f10a08bbac/.gitattributes?%2FTFOA2%2Fautomatic_fraude_detection%2Fresolve%2Fmain%2F.gitattributes=&etag=%22a6344aac8c09253b3b630fb776ae94478aa0275b%22` | CSV | O sistema pode comparar uma transação com padrões conhecidos de fraude para gerar um nível de risco |

> [!TIP]
> **Quer um dataset mais robusto?** Você pode utilizar datasets públicos do [Hugging Face](https://huggingface.co/datasets) relacionados a finanças, desde que sejam adequados ao contexto do desafio.

---

## Adaptações nos Dados

> Você modificou ou expandiu os dados mockados? Descreva aqui.

Não foram realizadas modificações nos dados mockados originalmente fornecidos. As bases foram utilizadas conforme disponibilizadas, mantendo sua estrutura e conteúdo original.
A única alteração realizada foi a inclusão de uma base adicional de detecção de fraude, obtida na plataforma Hugging Face. Essa base foi incorporada ao projeto com o objetivo de complementar a análise de padrões suspeitos e apoiar o agente virtual na identificação de possíveis fraudes financeiras.
A integração dessa base permite enriquecer o sistema com exemplos de comportamentos fraudulentos, contribuindo para que o agente ofereça alertas mais precisos e orientações preventivas aos usuários.

---

## Estratégia de Integração

### Como os dados são carregados?
> Descreva como seu agente acessa a base de conhecimento.

[ex:Os arquivos CSV e JSON são carregados no início da sessão do agente e utilizados como base de conhecimento durante as interações. Essas informações são acessadas para contextualizar atendimentos, analisar transações e personalizar orientações ao cliente. Também foi incluída uma base de detecção de fraude da plataforma Hugging Face para auxiliar na identificação de padrões suspeitos.]

### Como os dados são usados no prompt?
> Os dados vão no system prompt? São consultados dinamicamente?

Os dados não são inseridos diretamente no system prompt completo, mas são consultados dinamicamente durante a execução do agente. Quando o usuário faz uma pergunta ou relata uma situação suspeita, o agente acessa as bases de dados relevantes (como histórico de atendimento, perfil do investidor e transações) para obter informações que ajudem a contextualizar a resposta.
Esses dados são então utilizados para enriquecer o contexto do prompt enviado ao modelo de IA, permitindo gerar respostas mais personalizadas e orientadas à prevenção de fraudes. A base de detecção de fraude também auxilia na identificação de padrões suspeitos que podem indicar possíveis golpes.

---

## Exemplo de Contexto Montado

> Mostre um exemplo de como os dados são formatados para o agente.

```
CONTEXTO DO CLIENTE

Informações do Cliente:
Nome: João Silva
Perfil do Investidor: Moderado
Saldo disponível: R$ 5.000

Histórico de Atendimento:
- 10/10: Cliente relatou recebimento de e-mail suspeito solicitando atualização de dados bancários.
- 25/10: Cliente solicitou orientação sobre segurança em transferências via PIX.

Histórico de Transações Recentes:
- 01/11: Supermercado - R$ 450
- 03/11: Streaming - R$ 55
- 05/11: PIX - R$ 2.000 (destinatário não identificado)

Análise de Padrão:
- Cliente costuma realizar transações entre R$ 50 e R$ 500.
- Transferência atual está acima do padrão médio de gastos.

Sinalização de Risco:
- Possível transação suspeita detectada.
- Destinatário não presente no histórico do cliente.

INSTRUÇÕES PARA O AGENTE:
- Verificar se o cliente reconhece a transação.
- Alertar sobre possível fraude ou golpe.
- Orientar a não realizar novas transferências caso haja suspeita.
- Recomendar contato com o canal oficial do banco.

```
