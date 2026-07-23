# Ficha de Cadastro de Empresa (CNPJ) — envio por e-mail

Sistema para gerar a **Ficha de Cadastro de Empresa** (dados da empresa +
sócios + plano/tributação) em PDF, combinando dados extraídos do **Cartão
CNPJ** (Comprovante de Inscrição e de Situação Cadastral, emitido pela
Receita Federal) com informações digitadas à mão, e enviar essa ficha por
e-mail com o PDF anexado.

## Como usar

Abra `index.html` diretamente no navegador (não precisa de servidor nem
de instalação — é um app estático de página única).

1. **Importe o Cartão CNPJ em PDF**: o site lê o texto do PDF no navegador e
   tenta preencher automaticamente Contratante (Razão Social/Nome Fantasia),
   CNPJ, Endereço, Bairro, Cidade, Estado, CEP e CNAE Principal/Secundário.
   Como é busca por padrões no texto (não é um formato estruturado), **sempre
   confira os campos antes de gerar a ficha** — o site nunca bloqueia a
   geração mesmo se algum campo não for reconhecido.
2. **Preencha à mão** o que o Cartão CNPJ não traz: Nº do contrato, Contato
   Principal, Administração, Sócio 01/02 e capital de cada um, Vigência,
   Plano, Tributação e Valor do Capital.
3. **Baixe o PDF** da ficha ou **envie por e-mail** direto pelo site, com o
   PDF já anexado.

## Envio de e-mail (gratuito, sem servidor próprio)

O envio usa um backend gratuito em **Google Apps Script**, vinculado à sua
própria conta Gmail — sem custo, sem cartão de crédito, sem plano pago
(limite de 100 e-mails/dia numa conta Gmail comum). As instruções completas
de deploy estão em [`apps-script/README.md`](./apps-script/README.md).
Depois de publicado, cole a URL do Web App e o token na seção
**"Configuração do envio"** dentro de `index.html`.

## Estrutura do projeto

```
index.html              # aplicação (HTML + CSS + JS), sem dependências de build
apps-script/
  Code.gs               # backend de envio de e-mail (Google Apps Script)
  README.md             # passo a passo de deploy do Code.gs
```

Bibliotecas usadas (via CDN):
- [pdf.js](https://mozilla.github.io/pdf.js/) — leitura/extração de texto do Cartão CNPJ.
- [html2canvas](https://github.com/niklasvh/html2canvas) + [jsPDF](https://github.com/parallax/jsPDF) — geração do PDF da ficha.

## Publicando no Cloudflare Pages

O projeto é 100% estático (nenhum build necessário), então o deploy pelo
painel do Cloudflare Pages conectado ao GitHub é direto:

1. **Workers & Pages → Create → Pages → Connect to Git** e selecione este
   repositório (`jooaofelix/CadastroEmpresa`).
2. Em **Build settings**:
   - **Framework preset**: `None`.
   - **Build command**: deixe **vazio** (não há build).
   - **Build output directory**: `/` (raiz do repositório).
3. Clique em **Save and Deploy**.

Como o app está em `index.html` na raiz do repositório, a URL publicada
(`https://seu-projeto.pages.dev`) já abre a ficha diretamente — não precisa
navegar para nenhum subcaminho.

### Também funciona em qualquer outro serviço estático

GitHub Pages, Netlify, Vercel etc. — basta publicar a raiz do repositório,
sem configuração adicional.
