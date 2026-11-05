# Representação - painel de vendas

App para acompanhar pedidos, clientes, faturamento, trocas e reposição de produtos.
Stack: Next.js (App Router), Supabase (banco e login), Vercel (hospedagem).

## 1. Banco de dados (já feito)

O projeto Supabase `representacao-reconflex` já tem as tabelas, as views e as políticas de acesso.
Os arquivos `supabase/migrations/` servem de registro do que foi aplicado.

## 2. Criar seu usuário de login

No Supabase: **Authentication > Users > Add user > Create new user** (e-mail e senha).
Recomendo desativar o cadastro público em **Authentication > Sign In / Providers** (desligue "Allow new users to sign up").

## 3. Configurar o ambiente

```bash
cp .env.example .env.local
```

Preencha no `.env.local`:
- `NEXT_PUBLIC_SUPABASE_ANON_KEY`: em **Project Settings > API Keys**, chave pública (anon).
- `SUPABASE_SERVICE_ROLE_KEY`: na mesma tela, chave `service_role`. **Nunca exponha essa chave no navegador nem no GitHub.**

## 4. Rodar localmente

```bash
npm install
npm run dev
```

Acesse http://localhost:3000.

## 5. Subir para o GitHub e publicar na Vercel

1. Faça upload destes arquivos no repositório (sem a pasta `node_modules` e sem `.env.local`; o `.gitignore` já cuida disso).
2. Na Vercel: **Add New > Project**, selecione o repositório.
3. Em **Environment Variables**, adicione as três variáveis do `.env.example` com os valores reais.
4. Clique em Deploy.

## Como usar

- **Importar pedidos:** menu "Importar pedidos", envie o CSV no formato do `pedidos_itens_*.csv`. Pedidos com o mesmo número são atualizados.
- **Dashboard:** faturamento, trocas, clientes com 30+ dias sem comprar e produtos com reposição atrasada.
- **Cliente:** clique no nome em qualquer tabela. Veja os produtos comprados, a situação de reposição e os pedidos. Em cada venda, informe a data de faturamento (a previsão de entrega é preenchida em +2 dias), e a data de recebimento quando o cliente confirmar.

## Regras de cálculo

- **Sem compra há 30+ dias:** dias desde a última venda (trocas não contam).
- **Reposição atrasada:** com 2 ou mais compras do mesmo produto, alerta quando passa 1,5x o intervalo médio entre elas. Com uma compra só, alerta após 60 dias. Os dois números estão no arquivo `supabase/migrations/002_views_e_seguranca.sql` e podem ser ajustados.
