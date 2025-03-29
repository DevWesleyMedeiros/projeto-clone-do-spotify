# Anotações de Desenvolvimento React - Projeto Spotify Clone

## Fundamentos do React

### Diretórios e Navegação
- `./ ` nos diretórios representa o local atual onde estou

### Componentes
> "No React, todo componente é uma função que faz algo ou me retorna algo"

#### Formas de declarar componentes:
```jsx
// Sintaxe de Arrow Function
const App = () => {
  return <h1>Título</h1>;
}

// Sintaxe de função tradicional
function App() {
  return <h1>Título</h1>;
}
```
Ambas as formas funcionam perfeitamente no React.

### JavaScript no JSX
Para inserir expressões JavaScript dentro do JSX, utilize chaves `{}`.

```jsx
function App() {
  const nome = "Usuário";
  return <h1>Olá, {nome}!</h1>;
}
```

### Convenções de Nomenclatura

#### Componentes
- Seguem o **PascalCase**: primeira letra maiúscula e palavras emendadas
  - Exemplo: `NavBar`, `MusicPlayer`, `SongList`

#### Variáveis e Funções
- Seguem o **camelCase**: primeira letra minúscula e iniciais subsequentes maiúsculas
  - Exemplo: `userName`, `playMusic()`, `isPlaying`

#### Classes CSS (Metodologia BEM)
- **B**lock - **E**lement - **M**odifier
- Elementos dentro de um bloco são conectados por underline (`_`)
- Modificadores são conectados por traço duplo (`--`)
- Padrão: `className="bloco__elemento--modificador"`
- Nomes compostos para classes são separados por hífen (kebab-case)
  - Exemplo: `player__play-button--disabled`

### Importação e Exportação

- Exportação padrão (`export default`):
  ```jsx
  // Em Button.jsx
  export default function Button() { ... }
  
  // Em outro arquivo
  import BotaoCustomizado from './Button'; // Posso renomear livremente
  ```

- Exportação nomeada:
  ```jsx
  // Em utils.js
  export function formatTime() { ... }
  
  // Em outro arquivo
  import { formatTime } from './utils'; // Devo usar o nome exato entre chaves
  ```

- Componentes auto-fecháveis (self-closing tags):
  ```jsx
  <Componente />
  ```

### Box Model e Fragments

- No HTML, tudo é considerado uma caixa (box model)
- Tag vazia em React chama-se Fragment (`<></>`)
- Fragments permitem agrupar elementos sem adicionar nós extras ao DOM:
  ```jsx
  return (
    <>
      <h1>Título</h1>
      <p>Parágrafo</p>
    </>
  );
  ```

## Props

Props são como argumentos passados para componentes. Permitem transmitir informações de um componente pai para um filho.

```jsx
// Componente pai
function App() {
  return <MusicCard title="Bohemian Rhapsody" artist="Queen" />;
}

// Componente filho
function MusicCard(props) {
  return (
    <div>
      <h2>{props.title}</h2>
      <p>{props.artist}</p>
    </div>
  );
}
```

### StrictMode

No arquivo `main.jsx`, o `StrictMode` renderiza os componentes duas vezes em desenvolvimento para identificar problemas potenciais. Funciona apenas no ambiente de desenvolvimento.

```jsx
import React from 'react';
import ReactDOM from 'react-dom/client';
import App from './App';

ReactDOM.createRoot(document.getElementById('root')).render(
  <React.StrictMode>
    <App />
  </React.StrictMode>
);
```

## Destructuring

Permite extrair valores de objetos de forma mais concisa. Muito útil com props.

```jsx
// Sem destructuring
function MusicCard(props) {
  return <h2>{props.title}</h2>;
}

// Com destructuring
function MusicCard({ title, artist }) {
  return (
    <div>
      <h2>{title}</h2>
      <p>{artist}</p>
    </div>
  );
}
```

## Operador Ternário

É a forma mais eficiente para condicionais simples no JSX.

```jsx
// Condicional tradicional (não pode ser usada diretamente no JSX)
if (5 === "5") {
  console.log("É verdadeiro");
} else {
  console.log("É falso");
}

// Operador ternário - pode ser usado no JSX
5 === "5" ? console.log("É verdadeiro") : console.log("É falso");
```

Exemplo no JSX:
```jsx
function Playlist({ songs }) {
  return (
    <div>
      {songs.length > 0 ? (
        <ul className="song-list">
          {songs.map(song => <SongItem key={song.id} song={song} />)}
        </ul>
      ) : (
        <p>Nenhuma música encontrada.</p>
      )}
    </div>
  );
}
```

## Posicionamento dos Componentes
Por convenção, componentes são sempre declarados na parte final do código.

## Spread Operator
O operador spread (`...`) serve para copiar objetos e arrays. É útil porque variáveis de objetos são referências, não valores.

```jsx
// Objetos
const songData = { title: "Yesterday", artist: "The Beatles" };
const songCopy = { ...songData }; // Cria uma cópia real, não uma referência

// Arrays
const playlist = ["song1", "song2", "song3"];
const newPlaylist = [...playlist, "song4"]; // Cria nova array com item adicional
```

## Operador de Coalescência Nula

O operador `??` verifica se o valor à esquerda é `null` ou `undefined`.

```jsx
function ArtistName({ artist }) {
  const displayName = artist ?? "Artista Desconhecido";
  return <span>{displayName}</span>;
}
```

- Se `artist` for `null` ou `undefined`, retorna "Artista Desconhecido"
- Se `artist` tiver um valor, retorna o valor original

## React Router

Permite criar diferentes rotas (páginas) na aplicação.

```jsx
import { BrowserRouter, Routes, Route } from 'react-router-dom';
import Home from './pages/Home';
import Artists from './pages/Artists';
import Artist from './pages/Artist';
import Songs from './pages/Songs';
import Song from './pages/Song';
import Header from './components/Header';

function App() {
  return (
    <BrowserRouter>
      <Header />
      <Routes>
        <Route path="/" element={<Home />} />
        <Route path="/artists" element={<Artists />} />
        <Route path="/artists/:id" element={<Artist />} />
        <Route path="/songs" element={<Songs />} />
        <Route path="/song/:id" element={<Song />} />
      </Routes>
    </BrowserRouter>
  );
}
```

### Hooks de Roteamento

#### useLocation
import { useLocation } from "react-router-dom"; serve para saber onde eu estou dentro da minha aplicação ou na rota. Basta eu importá-lo no componente para mim descubrir sua rota
```jsx
import { useLocation } from "react-router-dom";

function NavBar() {
  const location = useLocation();
  
  return (
    <nav>
      <p>Você está em: {location.pathname}</p>
      {/* Link estará destacado se for a rota atual */}
      <Link className={location.pathname === '/' ? 'active' : ''} to="/">
        Home
      </Link>
    </nav>
  );
}
```

#### useParams
Utilizado para acessar os parâmetros da URL dentro de componentes em uma aplicação que utiliza o React Router para navegação. Ele permite acessar os valores que são passados como parâmetros nas rotas, como IDs ou qualquer outro dado que você queira capturar diretamente da URL.
```jsx
import { useParams } from "react-router-dom";

function ArtistPage() {
  const { id } = useParams(); // Obtém o ID da URL
  
  // Agora podemos usar o ID para buscar os dados do artista
  return <h1>Detalhes do Artista #{id}</h1>;
}
```

## Renderização em React
Um componente se re-renderiza quando:

1. Uma variável de estado (`useState`) é atualizada
2. As props do componente mudam
3. O componente pai é re-renderizado

### useState
Serve para criar e gerenciar estados em componentes funcionais. É uma forma de adicionar estado a componentes funcionais, de forma que o meu componente reanderize novamente quando o estado mudar.
import { useState } from 'react';
```jsx
import { useState } from 'react';

function Player() {
  // Declaração: [variável, função para atualizar a variável]
  const [isPlaying, setIsPlaying] = useState(false);
  
  return (
    <div>
      <button onClick={() => setIsPlaying(!isPlaying)}>
        {isPlaying ? "Pausar" : "Reproduzir"}
      </button>
    </div>
  );
}
```

Quando `setIsPlaying` é chamado:
1. O valor de `isPlaying` é atualizado
2. O componente `Player` é re-renderizado
3. Todos os componentes filhos que dependem desse estado também são re-renderizados

# Autor 
Wesley Medeiros da Rosa

- [Anotações de Desenvolvimento React - Projeto Spotify Clone](#anotações-de-desenvolvimento-react---projeto-spotify-clone)
  - [Fundamentos do React](#fundamentos-do-react)
    - [Diretórios e Navegação](#diretórios-e-navegação)
    - [Componentes](#componentes)
      - [Formas de declarar componentes:](#formas-de-declarar-componentes)
    - [JavaScript no JSX](#javascript-no-jsx)
    - [Convenções de Nomenclatura](#convenções-de-nomenclatura)
      - [Componentes](#componentes-1)
      - [Variáveis e Funções](#variáveis-e-funções)
      - [Classes CSS (Metodologia BEM)](#classes-css-metodologia-bem)
    - [Importação e Exportação](#importação-e-exportação)
    - [Box Model e Fragments](#box-model-e-fragments)
  - [Props](#props)
    - [StrictMode](#strictmode)
  - [Destructuring](#destructuring)
  - [Operador Ternário](#operador-ternário)
  - [Posicionamento dos Componentes](#posicionamento-dos-componentes)
  - [Spread Operator](#spread-operator)
  - [Operador de Coalescência Nula](#operador-de-coalescência-nula)
  - [React Router](#react-router)
    - [Hooks de Roteamento](#hooks-de-roteamento)
      - [useLocation](#uselocation)
      - [useParams](#useparams)
  - [Renderização em React](#renderização-em-react)
    - [useState](#usestate)


