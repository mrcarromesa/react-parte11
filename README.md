<h1>Rotas</h1>

- Utilizar o componente link para navegar entre as páginas:

```js
import { Link } from 'react-router-dom';
```

```js
<Link to={`/repository/${repository.name}`}>Detalhes</Link>
```

- No caso desse projeto, ele utilizar o seguinte formato de url:

```js
/proprietario/repositorio
```

- Então para não considerar a segunda barra ali como um outro endereço ou diretório
precisamos transformar em um caracter especial:

```js
encodeURIComponent(repository.name)
```

---

<h2>Ajustando a rota para receber parametro</h2>

- Para a rota receber parametros ajustamos dessa forma, no arquivo `src/routes.js`:

```js
<Route path="/repository/:repository" component={Repository} />
```

- O `:repository`, significa que estamos recebendo um novo parametro.

- Para receber a propriedade iremos modificar no arquivo `src/pages/Repository/index.js` :

```js
function Repository({ match }) {
  //...
}
```

- Desetruturamos o parametro `props`, para receber o `props.match`, ficando assim `{ match }`

- E para obter o valor que está vindo da url utilizamos isso:

```js
{ match.params.repository }
```

- E no caso como estamos utilizando o encodeURI, ao receber precisamos utilizar o decodeURI:

```js
{ decodeURIComponent(match.params.repository) }
```

---

<h2>Promise</h2>

- Temos a seguinte situação no arquivo `src/pages/Repository/index.js`, queremos buscar os dados do repositorio e as issues:

```js
// Nesse caso aqui ira realizar a primeira e so quando a primeira acabar
// ira executar a segunda, isso nao é interessante
const response = await api.get(`/repos/${repoName}`);
const issues = await api.get(`/repos/${repoName}/issues`);
```

- Porém no código acima irá executar a primeira linha e a segunda só será executada quando a primeira terminar.

- Nesse caso poderiamos realizar as duas chamada juntas, para tal utilizamos o recurso de `Promise`:

```js
const [repository, issues] = await Promise.all([
      api.get(`/repos/${repoName}`),
      api.get(`/repos/${repoName}/issues`)
    ]);
```

- O `Promise.all()` recebe um array, e retorna um array, podemos desestrutura esse array `const [repository, issues]`

---

<h2>PropTypes</h2>

- Para remover alguns erros gerados pelo eslint.

- Adicionar a biblioteca `prop-types`:

```bash
yarn add prop-types
```

