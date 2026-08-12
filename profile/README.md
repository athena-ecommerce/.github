# 🎨 Athena — Plataforma de Venda de Artes

**Projeto Integrador | Desenvolvimento de Aplicações Dinâmicas (DAD)**

Athena é uma plataforma onde **artistas cadastram e vendem suas obras** (quadros/artes) e **clientes navegam pelo catálogo, montam um carrinho e finalizam pedidos**. O produto conecta um frontend em JavaScript Vanilla a uma API própria em FastAPI, com carrinho persistente via Redis e upload de imagens via Cloudinary.

---

## 📦 Sobre o Produto

### O que o Athena entrega

✅ **Catálogo dinâmico de artes** com filtros (tipo de arte, nome, faixa de preço) e ordenação  
✅ **Página de detalhes** de cada obra, com imagem, descrição e preço  
✅ **Área do artista** para cadastrar, editar e remover suas próprias artes  
✅ **Carrinho persistente** (Redis, com expiração de 7 dias de inatividade)  
✅ **Checkout** com endereço de entrega (com busca automática por CEP via ViaCEP)  
✅ **Autenticação completa**: cadastro, login, refresh token e recuperação de senha por e-mail  
✅ **Responsividade** mobile e desktop  
✅ **Acessibilidade** alinhada a WCAG 2.1 AA

### Tema

Marketplace de **artes/quadros**, conectando artistas independentes a compradores interessados em obras autorais e releituras.

---

## 👥 Integrantes

| Nome                        | GitHub               | Função        | Responsabilidades                                          |
|-----------------------------|----------------------|---------------|------------------------------------------------------------|
| Nicolas Isepe Paz           | @NicolasIsepe        | Backend       | Endpoints de usuários e pagamentos                         |
| Samuel Pimenta Hironimus    | @samuelpimentah      | Backend       | Endpoints de artes e carrinho                              |
| Lorenzo Lima de Oliveira    | @LorenzoOliveira-git | Backend       | Autenticação, refresh tokens, recuperação de senha, e-mail |
| Raphaely Mendes Sales       | @raphaxnz            | Frontend      | Catálogo, carrinho, checkout (JS Vanilla)                  |
| Isabelly Vila Silva da Hora | @IsaDaHxra           | Frontend / QA | Acessibilidade, responsividade, testes                     |

> Atualize esta tabela com os nomes e usuários reais do grupo antes da entrega.

---

## 🛠️ Stack & Justificativas

### Frontend

| Tecnologia             | Justificativa                                                                                   |
|------------------------|-------------------------------------------------------------------------------------------------|
| **JavaScript Vanilla** | Requisito do projeto — sem React/Vue/Angular/jQuery                                             |
| **HTML5 semântico**    | `<header>`, `<nav>`, `<main>`, `<section>`, `<article>`, `<footer>`                             |
| **CSS3 puro**          | Layout responsivo com Flexbox/Grid, sem framework CSS pesado                                    |
| **Fetch API**          | Comunicação com o backend FastAPI via JSON e `multipart/form-data` (upload de imagem das artes) |

### Backend

| Tecnologia                     | Justificativa                                                                                                                                                    |
|--------------------------------|------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| **Python + FastAPI**           | Tipagem com Pydantic, documentação automática (Swagger/OpenAPI), boa curva de aprendizado                                                                        |
| **Uvicorn**                    | Servidor ASGI leve, padrão de mercado para FastAPI                                                                                                               |
| **SQLAlchemy 2.x (ORM)**       | Mapeamento objeto-relacional claro, `select()` moderno, Dirty Checking automático                                                                                |
| **PostgreSQL**                 | Banco relacional robusto, com suporte nativo a `CHECK`, `FOREIGN KEY` e integridade referencial — essencial para pedidos, pagamentos e usuários                  |
| **Redis**                      | Carrinho de compras como HASH (`carrinho:{id_usuario}`) com TTL de 7 dias — leitura/escrita muito mais rápida que ida ao Postgres a cada alteração de quantidade |
| **JWT (python-jose) + OAuth2** | Autenticação stateless com access token + refresh token                                                                                                          |
| **Cloudinary**                 | Hospedagem e otimização das imagens das artes, sem sobrecarregar o próprio servidor                                                                              |
| **fastapi-mail**               | Envio de código de recuperação de senha por e-mail                                                                                                               |

### Banco de Dados

Modelagem relacional normalizada cobrindo: usuários (clientes/artistas/administradores), endereços, telefones, competências, produtos (artes), imagens das artes, pedidos, itens do pedido, cartões, pagamentos e recuperação de senha. Ver detalhes completos em [`backend/README.md`](../../backend/README.md#🗄️-modelagem-de-dados).

### Acessibilidade (WCAG 2.1 AA)

✅ Imagens com `alt` descritivo (decorativas com `alt=""`)  
✅ Contraste de texto ≥ 4.5:1  
✅ Navegação completa por teclado (catálogo, carrinho e checkout)  
✅ Foco visível em todos os elementos interativos  
✅ HTML semântico validado via Lighthouse/axe DevTools  
✅ Formulários com `<label>` associada e mensagens de erro acessíveis  
✅ Nenhuma informação transmitida apenas por cor  
✅ Layout estável até 200% de zoom  
✅ `lang="pt-BR"` declarado no `<html>`  
✅ Lighthouse (Acessibilidade) com mediana 100, registrado em `lighthouse-report.md`

---

## 🚀 Instruções de Execução (ponta a ponta)

### Pré-requisitos

- **Python** 3.11+
- **PostgreSQL** 14+
- **Redis** (local via Docker, ou Redis Cloud / Upstash)
- **Git**
- Navegador moderno (Chrome, Firefox, Edge)

### 1️⃣ Clonar o repositório

```bash
git clone <url-do-repositorio>
cd Athena
```

### 2️⃣ Subir o Backend (FastAPI + Uvicorn)

```bash
cd backend

# Ambiente virtual
python -m venv venv
venv\Scripts\activate        # Windows
# source venv/bin/activate   # Linux/macOS

# Dependências
pip install -r requirements.txt

# Variáveis de ambiente
copy .env.example .env       # Windows
# cp .env.example .env       # Linux/macOS
# preencher DATABASE_URL, SECRET_KEY, REDIS_*, CLOUDINARY_* e ATHENA_EMAIL/PASSWORD

# Criar o banco (uma vez) executando o script SQL de backend/README.md
psql -U seu_usuario -d athena_db -f script_banco.sql

# Subir o servidor
uvicorn main:app --reload --port 8000
```

Backend disponível em `http://127.0.0.1:8000` — documentação interativa em `http://127.0.0.1:8000/docs`.

### 3️⃣ Subir o Frontend (JS Vanilla)

```bash
cd ../website

# Servir como arquivo estático, por exemplo:
python -m http.server 5500
# ou usar a extensão Live Server do VS Code
```

Frontend disponível em `http://localhost:5500`.

> Verifique se a origem do frontend está liberada em `origins` no `backend/main.py` (CORS).

### 4️⃣ Testar o fluxo completo

1. Abrir `http://localhost:5500`
2. Criar conta (`/auth/signup`) e logar (`/auth/login` ou `/auth/login-form`)
3. Navegar pelo catálogo de artes, aplicar filtros
4. Adicionar uma arte ao carrinho
5. Ir para o checkout, informar/selecionar endereço (com busca por CEP)
6. Finalizar o pedido e conferir a confirmação

**Console do navegador:** sem erros de CORS ou 404.  
**Network tab:** requisições para `http://127.0.0.1:8000/*` respondendo 200/201.

---

## 📡 Endpoints da API (resumo)

Base: `http://127.0.0.1:8000`

| Método | Endpoint | Descrição |
|--------|----------|-----------|
| `POST` | `/auth/signup` | Cadastro de usuário |
| `POST` | `/auth/login` | Login (JSON) |
| `POST` | `/auth/login-form` | Login (form, usado no Swagger) |
| `GET` | `/auth/refresh` | Renovar access token |
| `POST` | `/auth/resetpassword/email` | Enviar código de recuperação |
| `POST` | `/auth/resetpassword/validation` | Validar código |
| `POST` | `/auth/resetpassword/newpassword` | Definir nova senha |
| `GET` | `/arts/` | Listar artes (com filtros) |
| `GET` | `/arts/{id_produto}` | Detalhes de uma arte |
| `GET` | `/arts/artist/me` | Minhas artes (artista logado) |
| `POST` | `/arts/` | Cadastrar arte (artista) |
| `PUT` | `/arts/{id_produto}` | Editar arte (dono) |
| `DELETE` | `/arts/{id_produto}` | Remover arte (dono) |
| `GET` | `/cart/` | Ver carrinho |
| `POST` | `/cart/items` | Adicionar item ao carrinho |
| `DELETE` | `/cart/items/{art_id}` | Remover item do carrinho |
| `DELETE` | `/cart/` | Limpar carrinho |
| `POST` | `/purchase/` | Registrar pedido |
| `GET` | `/purchase/` | Listar pedidos do usuário |
| `GET` | `/user/profile` | Perfil do usuário |
| `POST` | `/user/adicionar-endereco` | Cadastrar endereço |
| `GET` | `/user/cep/{cep}` | Buscar endereço por CEP |

**Documentação detalhada de cada endpoint (payloads, respostas e erros):** [`backend/README.md`](../../backend/README.md)

---

## 📝 Uso de IA — Declaração de Transparência

### Ferramentas utilizadas

- **Claude (Anthropic)** — apoio na estruturação de endpoints do backend (artes, carrinho), organização de arquivos (`SCHEMAS`, `ROUTES`, `DEPENDENCIES`), revisão de bugs (ex: dependência de autenticação incompatível em `ROUTES/produtos.py`) e geração desta documentação.
- _Preencher outras ferramentas usadas pelo grupo (ChatGPT, Copilot etc.), se houver._

### Partes apoiadas por IA

| Componente                     | Ferramenta | O que foi gerado                                           | Revisão feita                                                                                                |
|--------------------------------|------------|------------------------------------------------------------|--------------------------------------------------------------------------------------------------------------|
| `rota /cart`                   | Claude     | Estrutura do carrinho baseada em Redis (HASH, TTL)         | Revisado, testado via Swagger, ajustado para o modelo de dados real                                          |
| `DEPENDENCIES/redis_client.py` | Claude     | Cliente Redis como dependency do FastAPI                   | Revisado; ajustado import para evitar estender o import circular pré-existente em `DEPENDENCIES/__init__.py` |
| `rota /arts`                   | Claude     | Diagnóstico do uso incorreto de `Depends(verificar_token)` | Corrigido manualmente para `verificar_token_oauth` após entender a causa                                     |
| Documentação (`README.md`)     | Claude     | Estruturação e redação da documentação técnica             | Conferida contra o código real do repositório                                                                |

_Preencher com as partes que o restante do grupo desenvolveu com apoio de IA (ex: frontend, autenticação)._

### Processo de revisão crítica

1. **Ler e entender** cada trecho gerado antes de aceitar
2. **Testar** no Swagger (`/docs`) e no fluxo real do frontend
3. **Adaptar** nomes, validações e mensagens de erro ao padrão do projeto (em português, consistente com o restante do código)
4. **Documentar** decisões técnicas relevantes (ex: por que o carrinho fica no Redis e não no Postgres)

Qualquer integrante do grupo é capaz de explicar o funcionamento de qualquer trecho do código, independentemente de quem escreveu ou de qual ferramenta apoiou.

---

## ♿ Acessibilidade — Evidências e Checklist

| #  | Critério                             | Status    | Evidência                           |
|----|--------------------------------------|-----------|-------------------------------------|
| 1  | Imagens com `alt` descritivo         | ⬜         | Código HTML                         |
| 2  | Contraste ≥ 4.5:1                    | ⬜         | WebAIM Contrast Checker             |
| 3  | Navegação completa por teclado       | ⬜         | Teste manual em carrinho e checkout |
| 4  | Foco visível                         | ⬜         | Inspeção visual                     |
| 5  | HTML semântico                       | ⬜         | Lighthouse / axe DevTools           |
| 6  | Labels associadas + erros acessíveis | ⬜         | Inspeção HTML / NVDA                |
| 7  | Nenhuma info só por cor              | ⬜         | Filtro de daltonismo                |
| 8  | Zoom até 200% sem quebrar            | ⬜         | Ctrl + scroll                       |
| 9  | `lang="pt-BR"` no `<html>`           | ⬜         | Inspeção HTML                       |
| 10 | Lighthouse Acessibilidade ≥ 90       | ⬜         | Execução em sala                    |

> Marcar cada item como concluído (✅) conforme validado, e anexar os resultados em **`lighthouse-report.md`** na raiz do repositório, com no mínimo 3 execuções (Chrome anônimo, sem extensões, categoria Acessibilidade, Desktop) e a mediana calculada.

---

## 🏷️ Controle de Versão

**Tag de entrega (congelar o MVP para a apresentação):**

```bash
git tag -a v1.0-pitch -m "MVP Athena - versão para apresentação"
git push origin v1.0-pitch
```

Branch de desenvolvimento atual referenciado: `feat/endpoint-arts` (endpoints de artes e carrinho). Consolidar na `main` antes de criar a tag final.

---

## 📂 Estrutura do Repositório

```
Athena/
├── backend/
│   ├── main.py
│   ├── requirements.txt
│   ├── .env.example
│   ├── README.md            # documentação técnica completa da API
│   ├── CORE/
│   ├── DEPENDENCIES/
│   ├── MODELS/
│   ├── ROUTES/
│   │   ├── auth.py          # /auth
│   │   ├── produtos.py      # /arts
│   │   ├── carrinho.py      # /cart
│   │   ├── purchase.py      # /purchase
│   │   └── user.py          # /user
│   └── SCHEMAS/
├── website/
│   ├── index.html
│   ├── landing_page/
│   ├── login/
│   ├── products/
│   ├── cart/
│   ├── payments/
│   ├── addresses/
│   ├── profile/
│   ├── publish/
│   ├── works/
│   └── shared/
├── .github/
│   └── README.md            # este arquivo
├── lighthouse-report.md
└── roteiro-pitch.md
```

---

## ⚠️ Avisos Importantes

1. **Entrega via GitHub:** o repositório é a fonte única de avaliação.
2. **Arguição técnica:** qualquer integrante pode ser questionado sobre qualquer parte do código.
3. **Prompt injection:** qualquer tentativa de manipular a IA de correção zera o grupo.
4. **Demo:** ter sempre um ambiente local funcionando como plano B (backend + frontend rodando na máquina, mesmo sem internet).

---

## 📋 Checklist Pré-Entrega

- [ ] Repositório público no GitHub
- [ ] `README.md` (este) e `backend/README.md` completos
- [ ] Backend rodando localmente via `uvicorn main:app --reload`
- [ ] Frontend rodando localmente (`website/`)
- [ ] Catálogo de artes dinâmico com filtros funcionando
- [ ] Carrinho + Checkout funcionais
- [ ] Autenticação (signup/login/refresh/recuperação de senha) funcionando
- [ ] Acessibilidade validada (Lighthouse ≥ 90, navegação por teclado testada)
- [ ] `lighthouse-report.md` com 3 execuções e mediana calculada
- [ ] `roteiro-pitch.md` (1 página)
- [ ] Commits distribuídos entre os integrantes (nenhum > 60–70%)
- [ ] Tag `v1.0-pitch` criada
- [ ] Declaração de uso de IA preenchida (seção acima)
- [ ] `.env` fora do repositório (`.gitignore`), apenas `.env.example` versionado

---

**Última atualização:** 12/08/2026
