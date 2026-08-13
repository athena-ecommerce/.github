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
| Samuel Pimenta Hironimus    | @samuelpimentah      | Backend       | Endpoints de artes e carrinho e documentação               |
| Lorenzo Lima de Oliveira    | @LorenzoOliveira-git | Backend       | Autenticação, refresh tokens, recuperação de senha, e-mail |
| Raphaely Mendes Sales       | @raphaxnz            | Frontend      | Catálogo, carrinho, checkout (JS Vanilla)                  |
| Isabelly Vila Silva da Hora | @IsaDaHxra           | Frontend      | Acessibilidade, responsividade, testes                     |

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

Modelagem relacional normalizada cobrindo: usuários (clientes/artistas/administradores), endereços, telefones, competências, produtos (artes), imagens das artes, pedidos, itens do pedido, cartões, pagamentos e recuperação de senha. Ver detalhes sobre as tabelas no README do repositório [`backend`](https://github.com/athena-ecommerce/backend).

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

## 🚀 Instruções de Execução
O deploy do projeto foi feito via Render e com banco de dados em nuvem no Aiven, portanto, é possível acessar o website via link público.

### 🐳 Rodar o Projeto Completo

Para acessar o projeto completo e funcional sem precisa configurar nada na própria máquina, acesse:
https://athena-frontend-83xp.onrender.com/index.html

## 📡 Endpoints da API (resumo)

Base: `http://127.0.0.1:8000`

| Método   | Endpoint                          | Descrição                      |
|----------|-----------------------------------|--------------------------------|
| `POST`   | `/auth/signup`                    | Cadastro de usuário            |
| `POST`   | `/auth/login`                     | Login (JSON)                   |
| `POST`   | `/auth/login-form`                | Login (form, usado no Swagger) |
| `GET`    | `/auth/refresh`                   | Renovar access token           |
| `POST`   | `/auth/resetpassword/email`       | Enviar código de recuperação   |
| `POST`   | `/auth/resetpassword/validation`  | Validar código                 |
| `POST`   | `/auth/resetpassword/newpassword` | Definir nova senha             |
| `GET`    | `/arts/`                          | Listar artes (com filtros)     |
| `GET`    | `/arts/{id_produto}`              | Detalhes de uma arte           |
| `GET`    | `/arts/artist/me`                 | Minhas artes (artista logado)  |
| `POST`   | `/arts/`                          | Cadastrar arte (artista)       |
| `PUT`    | `/arts/{id_produto}`              | Editar arte (dono)             |
| `DELETE` | `/arts/{id_produto}`              | Remover arte (dono)            |
| `GET`    | `/cart/`                          | Ver carrinho                   |
| `POST`   | `/cart/items`                     | Adicionar item ao carrinho     |
| `DELETE` | `/cart/items/{art_id}`            | Remover item do carrinho       |
| `DELETE` | `/cart/`                          | Limpar carrinho                |
| `POST`   | `/purchase/`                      | Registrar pedido               |
| `GET`    | `/purchase/`                      | Listar pedidos do usuário      |
| `GET`    | `/user/profile`                   | Perfil do usuário              |
| `POST`   | `/user/adicionar-endereco`        | Cadastrar endereço             |
| `GET`    | `/user/cep/{cep}`                 | Buscar endereço por CEP        |

**Documentação detalhada de cada endpoint (payloads, respostas e erros) acesse o README do backend** [`backend`](https://github.com/athena-ecommerce/backend))

---

## 📝 Uso de IA — Declaração de Transparência

### Ferramentas utilizadas

- **Claude (Anthropic)** — apoio na estruturação de endpoints do backend (artes, carrinho), organização de arquivos (`SCHEMAS`, `ROUTES`, `DEPENDENCIES`), revisão de bugs e geração desta documentação.
- **Codex (OpenAI)** — revisão e inserção de comentários explicativos no backend e no JavaScript do website, além da atualização desta documentação.

### Partes apoiadas por IA

| Componente                              | Ferramenta              | O que foi gerado                                                         | Revisão feita                                                                                                |
|-----------------------------------------|-------------------------|--------------------------------------------------------------------------|--------------------------------------------------------------------------------------------------------------|
| `rota /cart`                            | Claude                  | Estrutura do carrinho baseada em Redis (HASH, TTL)                       | Revisado, testado via Swagger, ajustado para o modelo de dados real                                          |
| `DEPENDENCIES/redis_client.py`          | Claude                  | Cliente Redis como dependency do FastAPI                                 | Revisado; ajustado import para evitar estender o import circular pré-existente em `DEPENDENCIES/__init__.py` |
| `rota /arts`                            | Claude                  | Diagnóstico do uso incorreto de `Depends(verificar_token)`               | Corrigido manualmente para `verificar_token_oauth` após entender a causa                                     |
| Documentação (`README.md`)              | Claude/Codex            | Estruturação, redação e atualização da documentação técnica              | Conferida contra o código real do repositório                                                                |
| Backend (`ROUTES`, `MODELS`, `SCHEMAS`) | Codex                   | Comentários curtos para explicar responsabilidades e fluxos              | Revisados para manter linguagem natural e não comentar operações óbvias                                      |
| Frontend (`website/`)                   | Codex                   | Correção de erros em JavaScript e comentários dos principais fluxos      | Revisados junto ao código existente                                                                          |
| Frontend (`HTML` e `CSS`)               | IA com revisão do grupo | Apoio na montagem de estruturas mais modernas, responsivas e organizadas | Adaptados ao visual e às necessidades do Athena                                                              |

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
| 1  | Imagens com `alt` descritivo         | ✅         | Código HTML                         |
| 2  | Contraste ≥ 4.5:1                    | ✅         | WebAIM Contrast Checker             |
| 3  | Navegação completa por teclado       | ✅         | Teste manual em carrinho e checkout |
| 4  | Foco visível                         | ✅         | Inspeção visual                     |
| 5  | HTML semântico                       | ✅         | Lighthouse / axe DevTools           |
| 6  | Labels associadas + erros acessíveis | ✅         | Inspeção HTML / NVDA                |
| 7  | Nenhuma info só por cor              | ✅         | Filtro de daltonismo                |
| 8  | Zoom até 200% sem quebrar            | ✅         | Ctrl + scroll                       |
| 9  | `lang="pt-BR"` no `<html>`           | ✅         | Inspeção HTML                       |
| 10 | Lighthouse Acessibilidade ≥ 90       | ✅         | Execução em sala                    |

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

**Última atualização:** 12/08/2026
