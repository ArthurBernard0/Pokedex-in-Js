# Pokedex-in-Js
Pokédex
Este projeto é uma aplicação web simples que utiliza a PokeApi para listar e exibir informações sobre Pokémon. A aplicação mostra uma lista de Pokémon com alguns detalhes.

Funcionalidades
Listagem de Pokémon: Exibe uma lista de Pokémon com base em uma chamada à API do Pokémon.
Detalhes dos Pokémon: Para cada Pokémon, mostra o número, nome, tipos e foto.

Tecnologias
JavaScript: Para lógica de front-end e manipulação da API.
HTML/CSS: Para a estrutura e estilo da página.
API do Pokémon: Utilizada para obter dados sobre os Pokémon.

Estrutura do Projeto
1. Classe Pokemon
Define a estrutura dos objetos Pokémon:

javascript
Copiar código
class Pokemon {
    number;
    name;
    type;
    types = [];
    photo;
}

2. Funções Principais
convertPokeApiDetailToPokemon(pokeDetail)
Converte os dados da API para um objeto Pokemon.

function convertPokeApiDetailToPokemon(pokeDetail) {
    const pokemon = new Pokemon();
    pokemon.number = pokeDetail.id;
    pokemon.name = pokeDetail.name;

    const types = pokeDetail.types.map((typeSlot) => typeSlot.type.name);
    const [type] = types;

    pokemon.types = types;
    pokemon.type = type;
    pokemon.photo = pokeDetail.sprites.dream_world.front_default;

    return pokemon;
}
-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------
pokeApi.getPokemonDetail(pokemon)
Obtém os detalhes de um Pokémon específico usando sua URL da API.

pokeApi.getPokemonDetail = (pokemon) => {
    return fetch(pokemon.url)
        .then((response) => response.json())
        .then(convertPokeApiDetailToPokemon);
};
-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------
pokeApi.getPokemons(offset = 0, limit = 10)
Obtém uma lista de Pokémon com base no offset e limite fornecidos.

pokeApi.getPokemons = (offset = 0, limit = 10) => {
    const url = `https://pokeapi.co/api/v2/pokemon?offset=${offset}&limit=${limit}`;
    return fetch(url)
        .then((response) => response.json())
        .then((jsonBody) => jsonBody.results)
        .then((pokemons) => Promise.all(pokemons.map(pokeApi.getPokemonDetail)))
        .then((pokemonsDetails) => pokemonsDetails);
};
-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------
convertPokemonToLi(pokemon)
Converte um objeto Pokemon para HTML.

function convertPokemonToLi(pokemon) {
    const typesHtml = Array.isArray(pokemon.types)
        ? pokemon.types.map(type => `<li class="type">${type}</li>`).join('')
        : '';

    return `
        <li class="pokemon">
            <span class="number">#${pokemon.number}</span>
            <span class="name">${pokemon.name}</span> 
            <div class="detail">
                <ol class="types">
                    ${typesHtml}
                </ol>
                <img src="${pokemon.photo}" 
                alt="${pokemon.name}">
            </div>
        </li>
    `;
}
