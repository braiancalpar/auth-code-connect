# CodeConnect — autenticação segura com React

Projeto desenvolvido durante o curso [React: implementando técnicas seguras de autenticação](https://cursos.alura.com.br/course/react-autenticacao-em-aplicacoes-react), da Alura.

O CodeConnect é uma aplicação React para conectar pessoas desenvolvedoras. Este repositório concentra o estudo de autenticação em aplicações React, combinando login tradicional, cadastro, tokens JWT, renovação de sessão, rotas protegidas e autenticação social com Google.

> Este é um projeto educacional e continua em desenvolvimento. A API usa JSON Server e um arquivo JSON como banco local, portanto não representa uma infraestrutura pronta para produção.

## Funcionalidades desenvolvidas

Com base na evolução registrada nos commits, o projeto possui:

- contexto global de autenticação com `AuthProvider` e hook `useAuth`;
- cliente HTTP responsável por incluir o access token nas requisições;
- login com e-mail e senha;
- validação dos formulários com React Hook Form e Zod;
- armazenamento seguro das senhas com hash no backend;
- cadastro de usuários com login automático após a criação da conta;
- rotas protegidas para impedir acesso sem autenticação;
- access token JWT de curta duração mantido em memória;
- refresh token armazenado em cookie `httpOnly`;
- renovação automática do access token e fila de requisições durante o refresh;
- recuperação da sessão ao atualizar a página;
- estado de carregamento enquanto a autenticação é restaurada;
- login social por meio do Google OAuth 2.0;
- página de callback para finalizar a autenticação com o Google;
- criação ou associação de usuários OAuth pelo endereço de e-mail;
- edição de nome, e-mail, senha, descrição e categorias do perfil;
- feed de pessoas desenvolvedoras e formulário de contato.

## Fluxo de autenticação

No login tradicional, a API valida as credenciais e devolve um access token, além de criar um refresh token em cookie `httpOnly`. O access token permanece apenas em memória e é usado pelo cliente HTTP nas chamadas autenticadas.

Quando a página é atualizada, o contexto de autenticação tenta recuperar a sessão por meio do refresh token. O projeto também agenda a renovação do access token antes de sua expiração.

No login social, a aplicação redireciona o usuário para o Google. Após a autorização, o backend processa o callback, localiza ou cria o usuário no banco local e retorna o fluxo para o frontend.

## Tecnologias

- React 19
- TypeScript
- Vite
- React Router
- React Hook Form
- Zod
- JSON Server
- JSON Web Token
- Google APIs / OAuth 2.0
- Tailwind CSS

## Pré-requisitos

Para executar o projeto, você precisa ter instalado:

- Node.js;
- npm;
- uma conta no Google Cloud, caso queira utilizar o login com Google.

O login tradicional com e-mail e senha pode ser utilizado sem configurar o Google Cloud. A conta e as credenciais do Google Cloud são obrigatórias somente para usufruir da parte OAuth do projeto.

## Configuração do Google OAuth

1. Acesse o [Google Cloud Console](https://console.cloud.google.com/).
2. Crie ou selecione um projeto.
3. Configure a tela de consentimento OAuth.
4. Crie uma credencial do tipo **ID do cliente OAuth 2.0** para uma aplicação Web.
5. Cadastre a seguinte URI de redirecionamento autorizada:

```text
http://localhost:3001/auth/google/callback
```

6. Crie um arquivo `.env` na raiz a partir do `.env.example` e informe as credenciais geradas:

```env
GOOGLE_CLIENT_ID=seu-client-id.apps.googleusercontent.com
GOOGLE_CLIENT_SECRET=seu-client-secret
PORT=3001
```

O arquivo `.env` contém informações sensíveis e já está configurado para não ser versionado.

## Como executar

1. Instale as dependências:

```bash
npm install
```

2. Configure o `.env` conforme a seção anterior se quiser usar o login com Google.

3. Em um terminal, inicie a API:

```bash
npm run api
```

4. Em outro terminal, inicie o frontend:

```bash
npm run dev
```

5. Acesse:

```text
http://localhost:5173
```

A API será executada em `http://localhost:3001`. Para o fluxo OAuth funcionar com a configuração atual, o frontend deve estar disponível na porta `5173`.

## Scripts disponíveis

| Comando | Descrição |
| --- | --- |
| `npm run dev` | Inicia o frontend em modo de desenvolvimento. |
| `npm run api` | Inicia a API local e o banco JSON. |
| `npm run build` | Verifica o TypeScript e gera a versão de produção. |
| `npm run lint` | Executa a análise estática do código. |
| `npm run preview` | Visualiza localmente a versão gerada pelo build. |

## Estrutura principal

```text
src/
├── features/
│   ├── auth/        # Contexto, serviços, formulários e validações de autenticação
│   ├── contact/     # Envio de mensagens para desenvolvedores
│   ├── devs/        # Listagem e dados das pessoas desenvolvedoras
│   ├── navigation/  # Navegação responsiva
│   └── profile/     # Edição e validação do perfil
├── lib/             # Configuração da API e cliente HTTP
└── pages/           # Páginas e callback OAuth

server.js            # API local, JWT, cookies, OAuth e atualização de perfil
database.json        # Banco de dados local usado pelo JSON Server
```

## Próximas implementações

Como continuidade do projeto, estão previstas:

- concluir o fluxo de logout, integrando o botão de saída ao contexto de autenticação e removendo o refresh token no backend;
- mover os segredos JWT e demais endereços configuráveis para variáveis de ambiente;
- aplicar autorização no backend às rotas que manipulam dados protegidos;
- adaptar cookies, CORS e URLs de callback para ambientes de produção;
- melhorar a geração de identificadores do banco local;
- adicionar testes automatizados para os fluxos de login, cadastro, refresh, OAuth e edição de perfil;
- substituir a API e o banco local por uma infraestrutura adequada antes de uma publicação em produção.

## Saiba mais

<details>
<summary>Informações originais do template React + TypeScript + Vite</summary>

### React + TypeScript + Vite

This template provides a minimal setup to get React working in Vite with HMR and some ESLint rules.

Currently, two official plugins are available:

- [@vitejs/plugin-react](https://github.com/vitejs/vite-plugin-react/blob/main/packages/plugin-react) uses [Babel](https://babeljs.io/) (or [oxc](https://oxc.rs) when used in [rolldown-vite](https://vite.dev/guide/rolldown)) for Fast Refresh
- [@vitejs/plugin-react-swc](https://github.com/vitejs/vite-plugin-react/blob/main/packages/plugin-react-swc) uses [SWC](https://swc.rs/) for Fast Refresh

### React Compiler

The React Compiler is not enabled on this template because of its impact on dev & build performances. To add it, see [this documentation](https://react.dev/learn/react-compiler/installation).

### Expanding the ESLint configuration

If you are developing a production application, we recommend updating the configuration to enable type-aware lint rules:

```js
export default defineConfig([
  globalIgnores(['dist']),
  {
    files: ['**/*.{ts,tsx}'],
    extends: [
      // Other configs...

      // Remove tseslint.configs.recommended and replace with this
      tseslint.configs.recommendedTypeChecked,
      // Alternatively, use this for stricter rules
      tseslint.configs.strictTypeChecked,
      // Optionally, add this for stylistic rules
      tseslint.configs.stylisticTypeChecked,

      // Other configs...
    ],
    languageOptions: {
      parserOptions: {
        project: ['./tsconfig.node.json', './tsconfig.app.json'],
        tsconfigRootDir: import.meta.dirname,
      },
      // other options...
    },
  },
])
```

You can also install [eslint-plugin-react-x](https://github.com/Rel1cx/eslint-react/tree/main/packages/plugins/eslint-plugin-react-x) and [eslint-plugin-react-dom](https://github.com/Rel1cx/eslint-react/tree/main/packages/plugins/eslint-plugin-react-dom) for React-specific lint rules:

```js
// eslint.config.js
import reactX from 'eslint-plugin-react-x'
import reactDom from 'eslint-plugin-react-dom'

export default defineConfig([
  globalIgnores(['dist']),
  {
    files: ['**/*.{ts,tsx}'],
    extends: [
      // Other configs...
      // Enable lint rules for React
      reactX.configs['recommended-typescript'],
      // Enable lint rules for React DOM
      reactDom.configs.recommended,
    ],
    languageOptions: {
      parserOptions: {
        project: ['./tsconfig.node.json', './tsconfig.app.json'],
        tsconfigRootDir: import.meta.dirname,
      },
      // other options...
    },
  },
])
```

</details>
