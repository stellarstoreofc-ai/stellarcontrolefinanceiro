# Nexus Control

Ferramenta para controlar contas a pagar, despesas, faturamento e lembretes de pagamento da empresa. Um único arquivo HTML, com login e dados sincronizados na nuvem via Supabase — acesse de qualquer computador com o mesmo login.

## Configuração inicial (uma vez só)

1. No painel do Supabase do projeto, abra **SQL Editor** e rode o script `supabase-setup.sql` (cria a tabela `financeiro_dados` e as regras de acesso).
2. Abra `index.html` no navegador, clique em **"Criar conta"** e cadastre seu email/senha de acesso.
3. Se o Supabase pedir confirmação por email, confirme pelo link recebido antes de tentar entrar (ou desative essa exigência em Authentication → Providers → Email → "Confirm email", se preferir acesso mais rápido para uso interno).
4. Em Authentication → URL Configuration, confira se o **Site URL** e as **Redirect URLs** apontam pro endereço onde o app está publicado (ex: `https://nexuscontrols.vercel.app`) — sem isso, os links de confirmação de email e de "Esqueci minha senha" não funcionam direito.

Esqueceu a senha? Na tela de login tem um link **"Esqueci minha senha"** — digite o email cadastrado e clique nele; o link do email recebido já leva pra tela de definir uma nova senha (não precisa mexer no painel do Supabase pra isso).

## Uso

Acesse **https://nexuscontrols.vercel.app** de qualquer computador, ou abra `index.html` diretamente no navegador (local). Não precisa de servidor nem build.

## Funcionalidades

Menu lateral, na ordem em que aparece:

- **Relatório mensal**: faturamento do mês (lançado manualmente), quanto entrou, quanto saiu e saldo. Tem duas abas — **Mensal** (com gráfico dos últimos 12 meses corridos) e **Anual** (soma o ano inteiro, navega por ano, e mostra um gráfico Jan-Dez daquele ano; clicar numa barra leva direto pro mês correspondente na aba Mensal).
- **Lembretes**: checklist mensal de pagamentos recorrentes (vem pré-cadastrado com contabilidade, ERP, imposto e INSS — dá pra editar, excluir ou adicionar outros), que reseta automaticamente a cada mês.
- **Contas**: cadastro de despesas/receitas com descrição, valor, vencimento, categoria e status. Painel com totais (a pagar, vencidas, pago no mês, saldo), filtros e busca.
- **Custo fixo por pedido**: calculadora que divide o custo fixo total do mês pela quantidade média de pedidos, com atalho pra puxar automaticamente o total de despesas da categoria "Fixas" pagas no mês.
- **Precificação de produtos**:
  - **Configurações padrão da precificação**, no topo da página: imposto (%), custo de embalagem (R$), custo fixo da operação (R$) e comissão do afiliado (%) — preenchidos uma única vez e usados automaticamente em toda calculadora e todo produto, sem precisar redigitar por marketplace. Um botão puxa o custo fixo calculado na página "Custo fixo por pedido".
  - **Calculadora rápida por marketplace**: uma aba por marketplace (Shopee, Mercado Livre, Shein, TikTok Shop — sempre coloridas), sem precisar salvar um produto. Cada aba mostra o essencial (custo do produto, custo de ads, meta de lucro, preço recomendado/de venda, lucro) e esconde as taxas específicas do marketplace num bloco recolhível "⚙️ Configurar taxas do marketplace".
  - **Meus produtos**: cadastro de produtos com foto (redimensionada automaticamente, só pra facilitar identificar visualmente na lista), marketplace, taxas e demais custos. Cada produto salvo tem um atalho "Ajustar taxas" pra trocar marketplace/taxa fixa/comissão sem abrir o formulário inteiro.
  - Ao escolher o "Marketplace", a taxa fixa e a comissão já vêm preenchidas com valores de referência: **Shopee** 18% + R$4 · **Shein** 18% + R$5 · **Mercado Livre** — tipo de anúncio Clássico 14%/Premium 19%, frete Grátis R$13,85/Por conta do comprador R$7,95 (ajustáveis em botões separados) · **TikTok Shop** 10% comissão + 6% frete + R$4 fixo (produto até R$50) ou 6% comissão + 6% frete + R$6 fixo (acima de R$50, faixa detectada **automaticamente** pelo preço de venda). Confirme sempre com as taxas reais da sua conta, pois variam por categoria/plano e mudam com frequência.
- **Backup manual**: exporta todos os dados em `.json` e permite restaurar depois — importante como segurança extra além da nuvem.
- **Menu lateral (mobile)**: em telas pequenas a navegação vira um menu "☰" que abre pela lateral.

## Onde ficam os dados

Tudo é salvo na tabela `financeiro_dados` do Supabase, vinculado ao seu usuário de login (protegido por Row Level Security — só quem faz login com a senha certa vê os dados). Ao logar em qualquer computador com o mesmo email/senha, os dados aparecem sincronizados. Cada login tem seus próprios dados, isolados dos demais.

## Deploy

Hospedado na **Vercel**, conectado a este repositório do GitHub — cada push na `main` já atualiza https://nexuscontrols.vercel.app automaticamente. Não há build step, é só HTML estático. O Netlify (drag-and-drop em app.netlify.com) também funciona como alternativa avulsa, se precisar.

Se o endereço do site mudar, atualize também em Supabase → Authentication → URL Configuration (Site URL / Redirect URLs) — senão o link de confirmação de email/recuperação de senha aponta pro lugar errado.
