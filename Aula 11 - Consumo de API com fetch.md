# 🌐 Aula: Trabalhando com `fetch` e Consumindo APIs no JavaScript (versão moderna com `async/await`)

## 🧠 O que é o `fetch`?

`fetch` é uma função nativa do JavaScript usada para **comunicar seu código com servidores** via HTTP — seja para **buscar** ou **enviar** dados.

* Totalmente **assíncrono**
* Baseado em **Promises**, mas você usa ele de forma muito mais elegante com `async/await`
* Não trava a aplicação enquanto espera resposta

---

## 📚 Por que Ler a Documentação da API?

Porque sem ela você está basicamente tentando ligar no número errado e ainda esperando que alguém atenda 😂.

Lendo a documentação você descobre:

* 📎 **Endpoints disponíveis**
* 📮 **Métodos HTTP permitidos**
* 🧾 **Formato dos dados enviados e recebidos (JSON?)**
* ❌ **Como tratar erros**
* 🔐 **Como autenticar (API Key, Bearer Token, etc)**

---

## ⚙️ Como funciona o `fetch` usando `async/await`?

### 🧪 Sintaxe Moderna:

```javascript
async function carregarDados() {
  try {
    const resposta = await fetch(url, options);
    const dados = await resposta.json();
    console.log(dados);
  } catch (erro) {
    console.error("Erro na requisição:", erro);
  }
}
```

> [!TIP]
> Fica muito mais limpo, legível e fácil de debugar.

---

## 🔁 Métodos HTTP e o CRUD

| CRUD   | Método HTTP | Significado    |
| ------ | ----------- | -------------- |
| Create | POST        | Criar          |
| Read   | GET         | Listar / obter |
| Update | PUT / PATCH | Atualizar      |
| Delete | DELETE      | Remover        |

---

# 🧪 Exemplos Práticos (versão async/await)

---

## ✅ Exemplo 1: GET – Listando Usuários

```html
<h1>Usuários</h1>
<ul id="listaUsuarios"></ul>

<script>
  async function carregarUsuarios() {
    try {
      const resposta = await fetch('https://jsonplaceholder.typicode.com/users');
      const usuarios = await resposta.json();

      const ul = document.getElementById('listaUsuarios');

      usuarios.forEach(user => {
        const li = document.createElement('li');
        li.textContent = `${user.name} - ${user.email}`;
        ul.appendChild(li);
      });
    } catch (erro) {
      console.error("Erro ao buscar usuários:", erro);
    }
  }

  carregarUsuarios();
</script>
```

---

## 📝 Exemplo 2: POST – Criando um Post

```html
<form id="formPost">
  <input id="titulo" placeholder="Título" required />
  <textarea id="conteudo" placeholder="Conteúdo" required></textarea>
  <button type="submit">Enviar</button>
</form>

<script>
  document.getElementById('formPost').addEventListener('submit', criarPost);

  async function criarPost(e) {
    e.preventDefault();

    const titulo = document.getElementById('titulo').value;
    const conteudo = document.getElementById('conteudo').value;

    try {
      const resposta = await fetch('https://jsonplaceholder.typicode.com/posts', {
        method: 'POST',
        headers: { 'Content-Type': 'application/json' },
        body: JSON.stringify({
          title: titulo,
          body: conteudo,
          userId: 1
        })
      });

      const resultado = await resposta.json();
      console.log("Post criado:", resultado);
    } catch (erro) {
      console.error("Erro ao criar post:", erro);
    }
  }
</script>
```

---

## 🎬 Exemplo 3: Consumindo a API OMDb

```javascript
async function buscarFilme() {
  try {
    const resposta = await fetch(`http://www.omdbapi.com/?t=Inception&apikey=SUA_API_KEY`);
    const filme = await resposta.json();

    console.log(filme.Title, filme.Year, filme.Poster);
  } catch (erro) {
    console.error("Erro ao buscar filme:", erro);
  }
}

buscarFilme();
```

> [!IMPORTANT]
> Substitua `SUA_API_KEY` por uma chave válida da OMDb (gratuita com cadastro no site deles).

---

## 🔎 Exemplo 4: Busca Dinâmica (OMDb)

```html
<form id="formBusca">
  <input id="filme" placeholder="Nome do filme" required />
  <button type="submit">Buscar</button>
</form>

<div id="resultado"></div>

<script>
  const API_KEY = 'SUA_API_KEY';

  document.getElementById('formBusca').addEventListener('submit', buscarFilme);

  async function buscarFilme(e) {
    e.preventDefault();
    const titulo = document.getElementById('filme').value;

    try {
      const resposta = await fetch(`http://www.omdbapi.com/?t=${titulo}&apikey=${API_KEY}`);
      const data = await resposta.json();

      const resultado = document.getElementById('resultado');

      if (data.Response === "True") {
        resultado.innerHTML = `
          <p><strong>Título:</strong> ${data.Title}</p>
          <p><strong>Ano:</strong> ${data.Year}</p>
          <img src="${data.Poster}" alt="Pôster">
        `;
      } else {
        resultado.innerHTML = '<p>Filme não encontrado 😢</p>';
      }
    } catch (erro) {
      console.error("Erro na busca:", erro);
    }
  }
</script>
```

---

# 🧩 Exercícios

Agora com a expectativa de usar `async/await` também 👇

---

### 1️⃣ Listar 10 tarefas (GET)

`https://jsonplaceholder.typicode.com/todos`

Mostre título + completed ✔️

---

### 2️⃣ Enviar formulário de comentários (POST)

`https://jsonplaceholder.typicode.com/comments`
Enviar nome + comentário → imprimir resposta no console.

---

### 3️⃣ Desafio Rick and Morty

Buscar personagem por nome:

`https://rickandmortyapi.com/api/character/?name=<nome>`

Mostrar:

* Nome
* Imagem
* Status (alive / dead / unknown)

---

# 📖 Documentações

* JSONPlaceholder: [https://jsonplaceholder.typicode.com/guide](https://jsonplaceholder.typicode.com/guide)
* OMDb: [http://www.omdbapi.com/](http://www.omdbapi.com/)
* Rick and Morty API: [https://rickandmortyapi.com/documentation](https://rickandmortyapi.com/documentation)
