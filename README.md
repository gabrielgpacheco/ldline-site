# LD Line — site estático (GitHub Pages)

Mirror fiel do WordPress `ldline.com.br` (tema PopularFX DentalCare + Pagelayer),
gerado em 2026-10-10 para desligar a hospedagem cPanel e servir grátis no GitHub Pages.

## Conteúdo

- `index.html` — Home
- `quem-somos/index.html` — Quem somos
- `tratamentos/index.html` — Tratamentos
- `contato/index.html` — Contato
- `wp-content/`, `wp-includes/` — CSS/JS/imagens/fontes (somente o usado)
- `CNAME` — contém `ldline.com.br` (não apagar)
- `.nojekyll` — impede o Jekyll de ignorar pastas `_`
- `404.html` — página de erro com menu

Total: ~49 arquivos, ~5,8 MB (limite Pages: 1 GB).

## O que foi removido do WordPress

- `wp-admin/`, `wp-json/`, `xmlrpc.php`, feeds RSS, oEmbed, `admin-ajax.php`
- Plugins PHP (`gosmtp`, `loginizer`, `siteseo`, `speedycache`, `backuply`) — só o CSS/JS
  estático necessário foi mantido (`pagelayer-bundle.css/js`, jQuery, Elementor CSS).
- `givecss.php?give=...` e `givejs.php?give=...` duplicados como
  `pagelayer-bundle.css` / `pagelayer-bundle.js` (GitHub Pages não executa PHP).
- Formulários WP: contato é via WhatsApp `https://wa.me/5521991031014` e tel
  `(21) 2221-5724`, já presentes no layout.

## Testar localmente

Pode abrir `index.html` com duplo clique (todos os links, CSS, JS e imagens
usam caminhos relativos `./` e `../`, então funciona via `file://`).

Ou sirva como um servidor estático (comportamento igual ao GitHub Pages):

```powershell
npx --yes serve "C:\Users\dayan\Downloads\migracao-sites\site-estatico"
# abrir http://localhost:3000/, /quem-somos/, /tratamentos/, /contato/
```

### Onde ficam as páginas

Cada página é um `index.html` dentro da sua pasta (formato exigido pelo
GitHub Pages para URLs limpas):

| Página | Arquivo local | URL publicada |
|---|---|---|
| Home | `index.html` | `/` |
| Quem somos | `quem-somos/index.html` | `/quem-somos/` |
| Tratamentos | `tratamentos/index.html` | `/tratamentos/` |
| Contato | `contato/index.html` | `/contato/` |

Para abrir sem servidor, dê duplo clique no `index.html` **dentro** de cada
pasta. Para navegação fiel (menu entre páginas), use o `npx serve` acima.

### Banner da homepage

Slider de fundo com `wp-content/uploads/2026/03/LD-Line.jpg` e `LD-Line-2.jpg`
(alterna a cada 10s via `pagelayer-bundle.js`). O arquivo
`wp-content/site-fallback.css` garante a primeira foto visível mesmo se o
JavaScript falhar.

> Nota: `canonical`, `og:url` e `twitter:url` continuam absolutos
> (`https://ldline.com.br/...`) de propósito — é o correto para SEO.
> Somente assets (`wp-content/`, `wp-includes/`) e links de navegação são
> relativos.

## Deploy no GitHub Pages (custom domain)

```powershell
Set-Location "C:\Users\dayan\Downloads\migracao-sites\site-estatico"
git init; git add .; git commit -m "LD Line static mirror"
git branch -M main
git remote add origin https://github.com/SEU-USUARIO/ldline-site.git
git push -u origin main
```

1. GitHub > repo > **Settings > Pages > Build and deployment**:
   - Source: **Deploy from a branch**
   - Branch: `main` / `(root)` > Save
2. **Custom domain**: `ldline.com.br` > Save (o arquivo `CNAME` já existe).
   Aguarde o check DNS e marque **Enforce HTTPS**.

## DNS (Registro.br) para `ldline.com.br`

```text
A @ 185.199.108.153
A @ 185.199.109.153
A @ 185.199.110.153
A @ 185.199.111.153
CNAME www SEU-USUARIO.github.io.
```

Propagação: 1–24h. Só cancele a hospedagem cPanel depois de
`https://ldline.com.br/` responder pelo GitHub com HTTPS válido.

## Atualizar depois (site quase nunca muda)

Editar o HTML direto ou re-gerar o mirror do site vivo antes de desligar.
Backup original guardado em: `../ldline/` (`public_html/` + `ldline_wp302.sql/`).
