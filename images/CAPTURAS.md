# Telas a capturar (mobile-first, janela do browser estreita)

Capturar rodando o app em `Claude/` (`npm run dev`), com a janela do navegador o
mais estreita possível (simular celular ~360–400px). Salvar como PNG nesta pasta
e substituir cada `<div class="ph">…</div>` pela `<img>` correspondente na página.

| Arquivo sugerido | Página | Tela |
|---|---|---|
| 01-cadastro-coordenador.png | 02-acesso | Cadastro de coordenador(a) de pesquisa |
| 02-login.png | 02-acesso | Tela de login (Entrar) |
| 03-definir-senha.png | 02-acesso | Ativação da conta: definição da senha (1º acesso, rota /ativar) |
| 04-termos-avaliacao.png | 03-modo-avaliacao | Termos do modo de avaliação |
| 05-uf.png | 04-entrar-operacao | Definição da Unidade da Federação |
| 06-advertencia-config.png | 04-entrar-operacao | Advertência de configuração |
| 07-boas-vindas.png | 05-boas-vindas-gestao | Boas-vindas do coordenador(a) |
| 08-painel-gestao.png | 05-boas-vindas-gestao | Painel de gestão (menu + saldos) |
| 09-aquisicao.png | 06-adquirir | Aquisição de entrevistas |
| 10-organizar.png | 07-organizar | Configurar a estrutura da pesquisa (possibilidades + cartões) |
| 11-entrevistadores.png | 08-entrevistadores | Entrevistadores da pesquisa (relação) |
| 12-distribuir.png | 09-distribuir | Distribuição de entrevistas |
| 13-configurar.png | 10-configurar | Configurar o questionário |
| 14-autorizar.png | 11-autorizar-iniciar | Confirmação de autorização de início |
| 15-resultados.png | 12-resultados | Exibição de resultados |
| 16-multiplas.png | 13-multiplas-pesquisas | Tratamento de mais de uma pesquisa |
| 17-trocar-senha.png | 02-acesso | Troca de senha (Registro de senha, com "Senha atual") |
| 18-menu-avaliacao.png | 03-modo-avaliacao | Menu das boas-vindas no trial (Sair do modo de avaliação) |
| 19-menu-gestao.png | 05-boas-vindas-gestao | Menu do painel de gestão, aberto (com Atualizar) |
| 20-entrevistador-expandido.png | 08-entrevistadores | Linha da relação aberta (dados + ações) |
| 21-precandidaturas.png | 10-configurar | Janela de pré-candidaturas (5 cargos) |
| 22-perfil-estratificacao.png | 12-resultados | Cartão Perfil com estratificação |
| 23-forma-pagamento.png | 06-adquirir | Modal "Forma de pagamento" (PIX/boleto) |

Opcional (enriquecem, mas não são citadas hoje): tela de resultado exibido,
modal "Criar nova pesquisa", modal "Redimensionar segmentos", menu colapsado.

## Recapturas realizadas (2026-07-02)
Após a remoção da funcionalidade **Metodologia** e a inclusão do item de menu
**Instalar aplicativo**, e o novo cartão **“Entrevistadores da pesquisa”** no
painel de gestão, estas duas telas foram **recapturadas** (via Playwright,
viewport 390×800 @2x = 780×1600, mesmo enquadramento das demais):
- **07-boas-vindas.png** — menu (expandido) agora inclui **Instalar aplicativo**
  entre *Editar dados do(a) coordenador(a)* e *Sair*. ✅
- **08-painel-gestao.png** — abaixo dos saldos, o novo cartão **“Entrevistadores
  da pesquisa Pesquisa01 - SC”** (entrevistadores com *Entrevistas realizadas* e
  *Entrevistas a realizar*). ✅

As mesmas imagens foram copiadas para a web page
(`../../web_page/assets/img/coord-boasvindas.png` e `coord-painel.png`).

O guia do **entrevistador** (`../entrevistador/images/04-boas-vindas.png`) **não**
precisou de recaptura: o menu está colapsado, então a troca *Metodologia →
Instalar aplicativo* não é visível na imagem.

## Observação sobre o formato das imagens
O guia do entrevistador embute as imagens em base64 (arquivo único portátil).
Aqui, por serem várias páginas, as imagens ficam como arquivos externos nesta
pasta (mais fácil de editar/atualizar). Se for preciso um pacote portátil por
página no futuro, dá para embutir depois.


## Recapturas de 2026-08-03 (revisão dos manuais)
TODAS as imagens foram refeitas: as anteriores mostravam a navbar **sem o ícone da
ponte**. Acrescentadas seis capturas (17 a 22) e trocado o sentido de duas:
`03-definir-senha.png` passa a ser a **ativação da conta** (dois campos, rota
`/ativar`) e `11-entrevistadores.png`, a **relação** de entrevistadores.

### Como as capturas de 03/08/2026 foram feitas
Playwright headless, `viewport 390×800` + `deviceScaleFactor 2` (PNG 780×1600),
`ignoreHTTPSErrors: true` (a interceptação de TLS desta máquina quebra o fetch do
IBGE na questão 1). Percurso: login `cliente@x.com` → termos de avaliação (telas
01–04, 17, 18, 22) → "Sair do modo de avaliação" → termos de operação →
advertência (06) → UF (05) → boas-vindas (07) → gestão: aquisição de 11 pacotes
(09, 23) → painel e menu (08, 19) → organizar (10) → entrevistadores (11, 20) →
distribuir (12) → configurar (13, 21) → autorizar (14) → resultados (15) →
múltiplas (16, por rota direta `#/cliente/multiplas`).

Armadilhas que custaram tempo (anotadas para a próxima vez):
- o **menu colapsável guarda estado** — o helper precisa checar `.navbar-collapse.show`
  em vez de simplesmente clicar no `.navbar-toggler`;
- "Autorizar início da pesquisa" vive DENTRO do menu: com o menu aberto atrás do
  modal, o clique em "Sim, autorizar início" só passa com `{ force: true }`, e a
  figura sai melhor capturando só `.modal-content`;
- a questão 1 (Município) busca as opções no IBGE: esperar o "Carregando opções…"
  sumir antes de responder;
- opções de múltipla escolha NÃO têm `role=checkbox` (só as de escolha única têm
  `role=radio`) — localizá-las por `[aria-pressed]`.
