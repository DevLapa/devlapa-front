 # DevLapa Front-end

Aplicação web do DevLapa, um sistema de apoio à operação e ao controle financeiro de bares e restaurantes. O front-end fornece a interface para autenticação, estoque, produtos, comandas, vendas, fornecedores, contas, relatórios, usuários, perfil e configurações.

> **Estado atual:** o projeto é um MVP em evolução. A visão de produto é disponibilizar a aplicação como SaaS, mas multi-tenancy, planos, cobrança recorrente e isolamento entre organizações ainda não estão implementados.

## Funcionalidades presentes

- Login com token JWT recebido da API.
- Rotas privadas e controle de acesso visual por perfil.
- Dashboard com indicadores de vendas, comandas e estoque.
- Gestão de produtos, categorias, estoque e fornecedores.
- Gestão de comandas e itens de comanda.
- Vendas e formas de pagamento.
- Contas a pagar e contas a receber.
- Relatórios com gráficos.
- Administração de usuários, perfil e preferências visuais.
- Temas e persistência de preferências no navegador.

As permissões efetivas são aplicadas pelo back-end. As proteções do React não substituem a autorização da API.

## Tecnologias

| Tecnologia | Uso |
| --- | --- |
| React 19 | Componentes e interface |
| Vite 8 | Desenvolvimento e build |
| React Router 7 | Navegação e rotas protegidas |
| Axios | Comunicação HTTP com a API |
| Tailwind CSS 4 | Estilos utilitários |
| Recharts | Gráficos dos relatórios |
| Lucide React e React Icons | Ícones |
| ESLint 9 | Análise estática |

## Organização

```text
src/
├── components/       # Layout e proteção de rotas
├── contexts/         # Autenticação e tema
├── pages/            # Telas e componentes específicos de cada módulo
├── services/         # Cliente Axios e autenticação
├── App.jsx           # Rotas da aplicação
├── main.jsx          # Ponto de entrada
└── index.css         # Estilos globais
```

## Pré-requisitos

- Node.js compatível com as dependências do projeto, recomendado Node.js 20 ou superior.
- npm.
- Back-end DevLapa executando localmente ou uma API publicada.

## Configuração local

1. Clone o repositório e entre na pasta:

   ```bash
   git clone https://github.com/DevLapa/devlapa-front.git
   cd devlapa-front
   ```

2. Instale as dependências:

   ```bash
   npm ci
   ```

3. Crie `.env.local` a partir de `.env.example`:

   ```env
   VITE_API_URL=http://localhost:8080
   ```

   Nunca publique tokens, senhas ou arquivos `.env.local`.

4. Inicie o servidor de desenvolvimento:

   ```bash
   npm run dev
   ```

O Vite informa no terminal a URL local, normalmente `http://localhost:5173`.

## Integração com a API

O cliente Axios usa `VITE_API_URL` como `baseURL` e envia o token armazenado no navegador no cabeçalho:

```text
Authorization: Bearer <token>
```

O login usa `POST /auth/login`. As telas também consomem módulos como `/api/comandas`, `/api/produtos`, `/api/estoque`, `/api/contas`, `/api/fornecedor`, `/api/categorias`, `/api/vendas` e `/api/formasdepagamento`.

Há endpoints chamados pelo front-end que ainda precisam ser alinhados ou implementados no back-end, incluindo cadastro, recuperação de senha, notificações e alguns caminhos de vendas, usuários, contas a receber e formas de pagamento. Consulte os controllers do back-end antes de integrar uma nova tela.

## Build e publicação

Para gerar os arquivos de produção:

```bash
npm run build
```

Para testar o build localmente:

```bash
npm run preview
```

O arquivo `vercel.json` configura o fallback para `index.html`, necessário para rotas do React Router na Vercel. Na Vercel, configure a variável:

```text
VITE_API_URL=https://sua-api.exemplo.com
```

O back-end não é executado na Vercel como um processo Spring Boot tradicional; ele deve ser publicado separadamente em uma plataforma de containers ou JVM.

## Verificações

```bash
npm run lint
npm run build
```

O build atual é gerado com sucesso. O lint ainda identifica problemas existentes em componentes, hooks e variáveis não utilizadas; eles devem ser corrigidos antes de configurar o lint como gate obrigatório de CI.

## Desenvolvimento

- Mantenha componentes de tela próximos à página que os utiliza.
- Centralize chamadas HTTP em serviços reutilizáveis.
- Não confie no perfil armazenado no navegador para autorização.
- Atualize a documentação quando adicionar ou alterar uma rota da API.
- Adicione testes para fluxos críticos antes de alterar autenticação ou operações financeiras.

## Visão SaaS

O produto pode evoluir para um serviço para múltiplos bares e restaurantes. Essa evolução exige, no front-end, cadastro de organizações, seleção de plano, estado da assinatura, convites de usuários e tratamento de contas suspensas. Nenhum desses fluxos de billing ou multi-tenancy está completo atualmente.

Requisitos futuros incluem:

- Identificar a organização ativa em todas as telas.
- Exibir apenas dados do tenant autenticado.
- Integrar planos e assinatura com o back-end.
- Tratar trial, inadimplência, cancelamento e limites por plano.
- Adicionar telemetria, tratamento consistente de erros e acessibilidade.

## Contribuição

Use branches com nomes descritivos, mantenha commits pequenos e valide `npm run build` antes de abrir um pull request. Não inclua arquivos `.env`, tokens, credenciais ou artefatos de `dist` no commit.
