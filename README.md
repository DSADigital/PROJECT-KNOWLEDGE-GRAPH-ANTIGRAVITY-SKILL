# Criar Skill: Project Knowledge Graph

## Objetivo

Implemente neste projeto um sistema de conhecimento e visualização arquitetural inspirado no Obsidian, especializado em relações entre arquivos de código-fonte.

O sistema deve permitir que agentes de IA compreendam a arquitetura do projeto, descubram dependências, consultem contexto relevante sob demanda e evitem ler arquivos desnecessários.

Preciso de duas entregas integradas:

1. Uma Skill funcional para o Antigravity, responsável por mapear, consultar e atualizar o conhecimento do projeto.
2. Um visualizador interativo de grafo, inspirado no Graph View do Obsidian, para explorar visualmente arquivos, módulos e dependências.

Não se limite a documentar como isso seria feito. Analise o repositório e implemente a solução adequada à arquitetura existente.

## 1. Análise inicial

Antes de implementar:

- Identifique as linguagens, os frameworks e as ferramentas disponíveis.
- Analise a estrutura do repositório e as configurações existentes do Antigravity.
- Verifique se já existe documentação arquitetural, indexação de código ou visualização de dependências.
- Escolha mecanismos compatíveis com o projeto para extrair relações reais entre arquivos.
- Evite adicionar dependências pesadas ou criar outro sistema se já existir uma solução aproveitável.

Preserve a aplicação existente e não modifique a lógica de negócio para implementar esse recurso.

## 2. Criar a Skill

Crie a Skill em:

`.agents/skills/project-knowledge-graph/SKILL.md`

Siga o formato oficial de Agent Skills aceito pelo Antigravity.

Use o nome `project-knowledge-graph` e uma descrição que permita ao agente identificar quando deve utilizar a Skill.

A Skill deve orientar o agente a:

- Descobrir os arquivos e módulos relevantes para uma tarefa.
- Consultar o grafo de dependências antes de ler grandes quantidades de código.
- Identificar dependências diretas e indiretas.
- Rastrear importações, exports, chamadas, interfaces, tipos e contratos.
- Localizar testes associados aos arquivos afetados.
- Consultar a documentação arquitetural e a memória técnica pertinente.
- Identificar os possíveis impactos de uma alteração.
- Atualizar o índice quando o código ou as relações mudarem.
- Consultar somente as informações necessárias para a tarefa atual.

A Skill deve utilizar scripts auxiliares quando isso trouxer benefícios reais.

Não coloque todo o conteúdo do projeto no `SKILL.md`. Mantenha as instruções principais concisas e delegue detalhes aos recursos auxiliares.

## 3. Criar um índice de conhecimento estruturado

Implemente um índice derivado do código, em formato leve e facilmente regenerável.

O índice deverá representar:

### Nós
- Arquivos.
- Diretórios ou módulos lógicos.
- Componentes e funções importantes, quando suportado.
- Interfaces, tipos e schemas relevantes.
- Testes.
- Documentos arquiteturais.

### Relações
- Importa e é importado por.
- Chama e é chamado por.
- Implementa uma interface.
- Utiliza um tipo ou contrato.
- Testa uma implementação.
- Depende de um módulo.
- Documenta uma funcionalidade.
- Possui uma relação arquitetural explicitamente documentada.

Cada relação deve conter, quando disponível:
- Arquivo de origem e destino.
- Tipo de relação.
- Evidência verificável.
- Localização aproximada no código.
- Grau de confiança.
- Indicação de como a relação foi descoberta.

Não invente conexões. Diferencie relações confirmadas pelo código de relações inferidas ou documentadas manualmente.

Não trate uma simples proximidade entre arquivos como prova de dependência.

## 4. Extrair relações de forma inteligente

Priorize métodos determinísticos de análise:

- Imports e exports.
- Referências de símbolos.
- Chamadas de funções.
- Tipos e interfaces.
- Rotas e handlers.
- Serviços e repositórios.
- Schemas e migrations.
- Testes e implementações.
- Referências entre documentos.

Utilize ferramentas de linguagem, servidores de linguagem ou parsers apropriados quando disponíveis.

Não tente analisar profundamente todas as linguagens com expressões regulares genéricas.

Quando uma relação não puder ser confirmada automaticamente, registre essa limitação em vez de fabricar uma conexão.

Implemente suporte prioritário às linguagens realmente utilizadas neste projeto e deixe a arquitetura preparada para adicionar outras linguagens posteriormente.

## 5. Criar o visualizador interativo

Construa uma interface visual navegável inspirada no Obsidian Graph View.

A interface deve ser implementada de acordo com a stack existente ou, se não houver uma aplicação apropriada, por meio de uma ferramenta local leve e isolada.

Recursos obrigatórios:

### Grafo global
- Mostrar os principais arquivos e módulos.
- Representar dependências por arestas direcionadas.
- Diferenciar tipos de relação visualmente.
- Permitir zoom, movimentação e seleção de nós.
- Evitar sobrecarga visual em projetos grandes.

### Grafo local
Ao selecionar um arquivo, permitir visualizar:
- Dependências diretas.
- Arquivos que dependem dele.
- Conexões indiretas por profundidade configurável.
- Testes relacionados.
- Documentos relevantes.

### Busca e filtros
- Buscar arquivos, símbolos e módulos.
- Filtrar por linguagem, diretório e tipo de relação.
- Exibir apenas conexões relevantes.
- Permitir destacar o caminho entre dois nós.
- Oferecer uma visão simplificada da arquitetura.

### Painel de detalhes
Ao selecionar um nó, apresentar:
- Caminho do arquivo.
- Responsabilidade resumida.
- Relações de entrada e saída.
- Símbolos relevantes, quando disponíveis.
- Referências aos arquivos relacionados.
- Data da última indexação.
- Origem e confiança das relações.

Sempre que possível, permitir abrir o arquivo correspondente no editor por meio de um mecanismo compatível com o ambiente.

Não construa apenas uma imagem estática ou um diagrama Mermaid. O objetivo é um visualizador interativo de navegação arquitetural.

## 6. Projetar para economia de tokens

O visualizador e a Skill devem funcionar como um sistema de recuperação seletiva de contexto.

Implemente, quando adequado:

- Resumos compactos por módulo.
- Índice de caminhos e símbolos.
- Consulta por arquivo, símbolo ou funcionalidade.
- Recuperação de dependências por profundidade.
- Respostas com referências para as fontes originais.
- Carregamento dos detalhes somente quando solicitados.
- Cache e atualização incremental do índice.
- Exclusão de diretórios irrelevantes, como dependências instaladas, builds e arquivos gerados.

O agente deve conseguir responder a perguntas como:

- Quais arquivos preciso analisar para modificar esta função?
- Quais módulos dependem deste componente?
- Quais testes devo executar?
- Que contratos podem ser afetados por esta alteração?
- Qual é o menor conjunto de arquivos necessário para compreender esta funcionalidade?

Sempre que possível, disponibilize uma interface de consulta que retorne resultados estruturados e compactos, em vez de despejar o conteúdo de arquivos inteiros.

Se um servidor MCP ou outra integração com o agente for realmente necessário, avalie sua compatibilidade e seu custo operacional antes de implementá-lo. Não adicione essa complexidade por padrão.

## 7. Atualização automática e confiabilidade

O índice não pode se tornar uma representação desatualizada do código.

Implemente um mecanismo adequado para:
- Detectar alterações nos arquivos.
- Reindexar apenas os arquivos afetados quando possível.
- Remover referências a arquivos excluídos.
- Recalcular relações afetadas por renomeações.
- Regenerar o índice integralmente quando necessário.
- Identificar falhas ou resultados parciais de indexação.

Utilize mecanismos compatíveis com o ambiente existente, como comandos explícitos de atualização, integração com o Git ou monitoramento de arquivos.

Não exija que o modelo releia o repositório inteiro sempre que um arquivo mudar.

O código-fonte deve continuar sendo a fonte de verdade.

## 8. Persistência e organização

Organize os arquivos de forma clara. Como referência, considere:

- `.agents/skills/project-knowledge-graph/SKILL.md`
- `docs/knowledge-graph/`
- Um índice estruturado e regenerável.
- Scripts de indexação e consulta.
- A interface do visualizador.
- Testes para a extração de relações.

Adapte os caminhos à arquitetura real. Não crie uma estrutura rígida se o projeto já tiver convenções melhores.

Evite duplicar o conteúdo do código em Markdown. Armazene principalmente metadados, relações, resumos e referências.

## 9. Segurança e desempenho

- Respeite as exclusões e os arquivos ignorados pelo Git, com possibilidade de configuração.
- Não indexe segredos, credenciais, arquivos `.env` ou conteúdo sensível.
- Não envie o código do projeto para serviços externos.
- Não introduza serviços remotos obrigatórios.
- Não execute comandos destrutivos.
- Não modifique arquivos de negócio fora do escopo.
- Evite indexar dependências de terceiros indiscriminadamente.
- Não gere milhares de documentos redundantes.
- Não afirme que houve economia de tokens sem medições.

## 10. Validar o resultado

Crie testes para verificar:
- Extração de dependências conhecidas.
- Identificação de arquivos relacionados.
- Tratamento de arquivos removidos e renomeados.
- Atualização incremental.
- Funcionamento das consultas.
- Consistência dos dados exibidos no grafo.
- Exclusão de arquivos sensíveis.
- Comportamento com projetos maiores.

Execute os testes pertinentes e corrija os problemas encontrados.

Ao finalizar, apresente:
- Arquivos criados e modificados.
- Como ativar a Skill no Antigravity.
- Como iniciar e utilizar o visualizador.
- Como atualizar o índice.
- Linguagens e tipos de relação suportados.
- Limitações conhecidas.
- Resultados reais dos testes.

## 11. Integração obrigatória com o grafo

Não basta criar a Skill e o visualizador. Configure o Antigravity para utilizar o índice do grafo durante o trabalho de desenvolvimento.

1. Adicione às instruções permanentes do projeto uma regra curta que determine a consulta ao grafo antes de investigar alterações de código, bugs ou refatorações.
2. Configure a Skill `project-knowledge-graph` para ser acionada quando a tarefa envolver análise, navegação, alteração ou depuração de código.
3. Disponibilize uma ferramenta de consulta eficiente que retorne dependências, arquivos relacionados, símbolos relevantes e testes associados.
4. Antes de modificar código, consulte o grafo para identificar os arquivos envolvidos e expanda a investigação conforme necessário.
5. Se o índice estiver ausente, desatualizado ou incompleto, atualize-o ou consulte diretamente o código-fonte.
6. Se a ferramenta de consulta falhar, continue a investigação por outros meios, sem inventar relações.
7. Não leia o grafo completo em cada tarefa. Consulte somente os nós, as relações e os resumos relevantes.
8. Após alterações que afetem dependências, atualize o índice conforme necessário.

### Requisito de validação

Execute uma tarefa de teste que solicite a análise de uma funcionalidade existente. Verifique se o agente consulta efetivamente o índice, identifica os arquivos relacionados e utiliza esses resultados na análise.

Não considere a integração concluída apenas porque a Skill e o visualizador foram criados. Se o ambiente não permitir consultas automáticas, documente a limitação e implemente o mecanismo compatível mais próximo.

## Critério de conclusão

A solução estará concluída quando eu puder visualizar e explorar as relações reais do projeto, selecionar um arquivo e descobrir suas dependências, e utilizar a Skill para recuperar contexto relevante antes de alterar o código.

O sistema deve priorizar precisão, atualização, navegação intuitiva e economia de tokens, sem sacrificar a investigação necessária para realizar alterações seguras.

Analise o projeto e implemente a solução funcional mais simples que satisfaça esses requisitos.
