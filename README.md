<!DOCTYPE html>
<html lang="en">
  <head>
    <title>Busca de filmes</title>
    <meta charset="UTF-8" />
    <meta name="viewport" content="width=device-width" />
    <link rel="stylesheet" href="styles.css" />
  </head>
  <body>
    <h1>MSB</h1>
    <form>
      <input name="buscar" placeholder="Digite o nome do filme">
      <button>Buscar</button>
    </form>
    <div class="movieList"></div> 
    <button id="loadMoreBtn" style="display: none;">Load More</button>
  </body>
  <script>
    const formBusca = document.querySelector("form");
    const list = document.querySelector("div.movieList");
    const loadMoreBtn = document.getElementById("loadMoreBtn");

    let chave = 'ee49f034';
    let paginaAtual = 1;

    formBusca.onsubmit = (evento) => {
      evento.preventDefault();
      paginaAtual = 1;
      searchMovies();
    };

    function searchMovies() {
      const busca = formBusca.elements.buscar.value;

      if (busca === "") {
        alert("É necessário digitar algo");
        return;
      }

      fetch(`http://www.omdbapi.com/?s=${busca}&apikey=${chave}&page=${paginaAtual}`)
        .then(resultado => resultado.json())
        .then(json => {
          mostrarFilmes(json);

          if (json.Search && json.Search.length > 0) {
            loadMoreBtn.style.display = "block";
          } else {
            loadMoreBtn.style.display = "none";
          }
        });
    }

    function mostrarFilmes(json) {
      if (paginaAtual === 1) {
        list.innerHTML = "";
      }

      json.Search.forEach(element => {
        let filmes = document.createElement("div");
        filmes.classList.add("filmes");

        let googleSearchLink = `https://www.google.com/search?q=${encodeURIComponent(element.Title)}`;

        filmes.innerHTML = `<a href="${googleSearchLink}" target="_blank"><img src="${element.Poster}"/></a><h2>${element.Title}</h2>`;
        list.appendChild(filmes);
      });
    }

    loadMoreBtn.addEventListener("click", () => {
      paginaAtual++;
      searchMovies();
    });
  </script>
</html>