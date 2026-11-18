# Shuffle Real

Página web que embaralha uma playlist do Spotify com aleatório **uniforme** (Fisher-Yates com
`crypto.getRandomValues`) e manda o Spotify tocar nessa ordem, com o shuffle nativo desligado.
Faixas que você ainda não ouviu vêm primeiro; o histórico é alimentado pelas suas últimas
reproduções reais no Spotify e zera sozinho quando a playlist inteira já tocou.

Funciona com o app oficial do Spotify no iPhone. Exige **Spotify Premium** (a API só deixa
controlar a reprodução de contas Premium).

## Como funciona

1. Lê todas as faixas da fonte escolhida (uma playlist ou as Músicas Curtidas).
2. Embaralha com permutação uniforme: toda ordem tem a mesma chance.
3. Escreve a ordem numa playlist privada sua chamada "Shuffle Real (fila)", recriada a cada uso.
4. Desliga o shuffle do Spotify e começa a tocar essa playlist do início.

## Configuração (uma vez só)

### 1. Publicar a página

A página precisa estar em HTTPS para o login do Spotify funcionar no iPhone. O jeito mais fácil
é o GitHub Pages:

1. Crie um repositório no GitHub (pode ser privado? **não**: GitHub Pages gratuito exige
   repositório público. O código não tem segredo nenhum, só a sua Client ID fica salva no
   navegador).
2. Envie o `index.html` para a raiz do repositório.
3. Settings → Pages → Source: "Deploy from a branch", branch `main`, pasta `/ (root)`.
4. Anote a URL, algo como `https://SEU-USUARIO.github.io/NOME-DO-REPO/`.

Qualquer outra hospedagem estática com HTTPS (Netlify, Vercel, Cloudflare Pages) também serve.

### 2. Criar o app no Spotify

1. Entre em <https://developer.spotify.com/dashboard> com a sua conta Spotify.
2. "Create app". Nome e descrição livres.
3. **Redirect URI**: a URL exata da página, com a barra final, por exemplo
   `https://SEU-USUARIO.github.io/NOME-DO-REPO/`. A página mostra o valor exato a usar.
4. Em "APIs used" marque **Web API**. Salve.
5. Copie o **Client ID** (não precisa do Client Secret; o login usa PKCE).

O app fica em "Development mode", que permite até 25 usuários. Para uso pessoal basta.

### 3. No iPhone

1. Abra a URL no Safari.
2. Cole o Client ID e toque em "Entrar com Spotify". Autorize.
3. Compartilhar → "Adicionar à Tela de Início" para virar um ícone.

## Uso

1. Abra o Spotify e deixe tocando (ou pausado) qualquer coisa, para ele existir como dispositivo.
2. Abra o Shuffle Real, escolha a fonte e toque em "Embaralhar e tocar".
3. Não ligue o botão de aleatório do Spotify; a ordem já está embaralhada.

Para repetir com outra ordem, toque de novo. "Zerar histórico" faz a fonte voltar a tratar tudo
como não ouvido.

## Limites

- Desde fevereiro de 2026 a API só entrega o conteúdo de playlists **suas ou em que você
  colabora**. Playlists editoriais do Spotify ou de outras pessoas dão erro 403; salve uma cópia
  na sua biblioteca ("Adicionar a outra playlist" → nova playlist) e use a cópia.
- O Spotify limita apps em modo de desenvolvimento a 5 usuários autorizados e exige Premium.

- O histórico só sabe o que o Spotify informa nas últimas 50 reproduções. Se você ouvir mais
  de 50 faixas entre dois usos do app, as mais antigas não entram no histórico.
- Smart Shuffle e recomendações do Spotify não interferem: a reprodução é de uma playlist
  comum, em ordem.
- Faixas locais (arquivos do seu computador) são ignoradas, pois a API não as controla.
