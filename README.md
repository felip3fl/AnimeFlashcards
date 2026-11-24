<h3 align="center"> <img src="https://raw.githubusercontent.com/felip3fl/felip3fl/d3fc7831c647a88ff79c6d6d8f1041014bb71028/Material/FL/FLmetroDefault.svg" width="160px" align="center" ><BR><BR>AnimeFlashcards
<BR><BR>
  <img src="https://img.shields.io/badge/STATUS-EM%20DESENVOLVIMENTO-ff265c" alt="Status: EM DESENVOLVIMENTO">
  <img src="https://img.shields.io/badge/PROJECT%20VERSION-1.0.0-ff262f" alt="Project version: 1.0.0">
<BR><BR>
</h3>

## 📃 Descrição do projeto

<p align="justify">
O <strong>AnimeFlashcards</strong> (FlashCards de Estudo) é uma aplicação web interativa em formato <strong>PWA (Progressive Web App)</strong> desenvolvida com HTML5, CSS3 e JavaScript puro para treinar a memorização de personagens de animes — além de incluir personagens de jogos selecionados. Cada flashcard exibe a imagem do personagem na parte frontal e, ao ser clicado, realiza uma animação 3D de rotação revelando o nome completo, uma descrição curta e a obra correspondente. A aplicação conta com embaralhamento automático, painel de estatísticas e barra de progresso em tempo real, criação de novos cards personalizados com upload de imagem e suporte à instalação como aplicativo.
</p>

<br>

## :hammer: Funcionalidades do projeto

- `Flashcards interativos em 3D`: clique em qualquer card para virá-lo suavemente e revelar o nome do personagem, sua descrição e a obra/categoria, com exibição de um aviso rápido (tooltip) do nome revelado.
- `Catálogo pré-cadastrado de personagens`: deck inicial com personagens de diversas obras, incluindo *Dan Da Dan*, *Hellsing Ultimate*, *Chainsaw Man*, *Demon Slayer*, *JoJo's Bizarre Adventure*, *Jujutsu Kaisen*, *Mob Psycho 100*, *Clannad*, *Berserk*, *Evangelion*, *Ousama Ranking*, *Black Clover*, *Hellblade* e *Red Dead Redemption*.
- `Estatísticas e barra de progresso`: acompanhamento em tempo real da quantidade total de cards, cards revelados, cards restantes e percentual concluído.
- `Embaralhar e reiniciar`: os flashcards são embaralhados automaticamente ao iniciar a aplicação, com botões para reembaralhar o deck ou reiniciar (ocultar todos os cards revelados) a qualquer momento.
- `Criação de novos cards`: modal para adicionar flashcards personalizados durante a sessão, com upload e pré-visualização de imagem local, campo obrigatório de nome/resposta e campos opcionais de descrição e categoria.
- `Aplicativo instalável (PWA)`: suporte a instalação via `Web App Manifest`, banner de instalação adaptado para navegadores compatíveis e instruções para iOS/Safari, além de `Service Worker` para cache dos arquivos principais (App Shell).
- `Interface responsiva`: layout em grade adaptável para diferentes tamanhos de tela, de computadores a dispositivos móveis.

<br>

## 📁 Acesso ao projeto

Você pode acessar o código-fonte do projeto diretamente no [repositório do GitHub](https://github.com/felip3fl/AnimeFlashcards) ou baixá-lo em formato `.zip` / cloná-lo via Git:

```bash
git clone https://github.com/felip3fl/AnimeFlashcards.git
```

<br>

## 🛠️ Abrir e rodar o projeto

Por se tratar de uma aplicação web estática sem necessidade de compilação ou instalação de dependências, há duas formas simples de executá-la:

1. **Abertura direta no navegador**
   - Clone ou baixe e extraia os arquivos do repositório.
   - Abra o arquivo `index.html` diretamente no navegador de sua preferência.

2. **Execução em servidor local (recomendado para testar recursos de PWA)**
   - Para utilizar o `Service Worker` e o prompt de instalação do aplicativo, sirva a pasta do projeto via servidor HTTP local — por exemplo, utilizando a extensão **Live Server** no Visual Studio Code ou executando um dos comandos abaixo na raiz do projeto:

```bash
# Usando Python
python -m http.server 8000

# Ou usando Node.js
npx serve .
```

   - Em seguida, acesse o endereço exibido no terminal (por exemplo, `http://localhost:8000`) pelo navegador.

<br>  

## ✔️ Técnicas e tecnologias utilizadas
<p align="justify">
<img width="80" src="https://cdn.jsdelivr.net/gh/devicons/devicon/icons/html5/html5-original.svg" alt="HTML5">
&nbsp;&nbsp;<img width="80" src="https://cdn.jsdelivr.net/gh/devicons/devicon/icons/css3/css3-original.svg" alt="CSS3">
&nbsp;&nbsp;<img width="80" src="https://cdn.jsdelivr.net/gh/devicons/devicon/icons/javascript/javascript-original.svg" alt="JavaScript">
&nbsp;&nbsp;<img width="80" src="https://cdn.jsdelivr.net/gh/devicons/devicon/icons/git/git-original.svg" alt="Git">
&nbsp;&nbsp;<img width="80" src="https://raw.githubusercontent.com/felip3fl/felip3fl/1a6a66b6a143aab342cf2df18f56d8c1c7e6c8fb/Material/Icon/vscode.svg" alt="Visual Studio Code">
</p>

- `HTML5`
- `CSS3 (CSS Grid, Variáveis CSS, Animações 3D e Design Responsivo)`
- `JavaScript (ES6+ / Vanilla JS)`
- `Progressive Web App (Web App Manifest e Service Worker)`
- `Web APIs (FileReader API e LocalStorage)`
- `Google Fonts (Playfair Display e DM Sans)`
- `Git`
- `Visual Studio Code`

<br>  

## Autor

| [<img src="https://github.com/felip3fl/felip3fl/blob/main/Material/Nick/nick1.jpg?raw=true" width=115><br><sub>Felipe Lima</sub>](https://github.com/felip3fl) | 
| :---: 

<br>

## Licença

<i><h6>Todos os direitos reservados. O autor permite apenas a visualização do código, sendo proibida qualquer utilização do mesmo, no todo ou em parte.</h6></i>
