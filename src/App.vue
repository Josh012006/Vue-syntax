<template>

    <!-- L'affichage conditionnelle avec v-show. On met à l'intérieur une expression booléenne -->
    <div v-show="count >= 5 || count <= -5" class="message green">
        Bravo vous avez cliqué plus de 5 fois.
    </div>

    <!--
        On peut utiliser v-hide poiur l'effet contraire de v-show
        v-if pour le même effet que v-show sauf que v-if supprime carrément
        l'élément du DOM alors que v-show ne fait que le mettre en display hidden
        A un v-if, on peut associer un v-else-if ou un v-else pour implémenter une 
        certaine logique.
    -->

    <div v-if="count >= 10 || count <= -10" class="message green">
        Bravo vous avez cliqué plus de 10 fois.
    </div>
    <div v-else :style="{color: (count <= -5 || count >=5) ? 'orange' : 'red' }" class="message red">
        Vous avez cliqué moins de 10 fois.
    </div>


    <!-- On peut aussi avoir un attribut dynamique à l'aide v-bind:nom_attribut
        Mais on peut égaement juste mettre un : devant l'attribut, c'est la forme réduite.-->
    <p v-bind:id="`p-${count}`" class="counter">Compteur: {{ count }}</p>

    <div class="buttons">

        <!-- @evenement pour un gestionnaire d'événements. 
            On met entre guillemets le nom de la fonction 
            JavaScript définie dans la balise script. -->
        <button @click="decrement" class="decr">-</button>

        <!-- On peut aussi utiliser la syntaxe v-on:evenement -->
        <button v-on:click="increment" class="incr">+</button>

    </div>


    <br>
    <hr>
    <br>

    <!-- Les boucles -->
    <ul>
        <li v-for="movie in movies" :key="`${movies.indexOf(movie)}`">
            {{ movie }}
            <button class="button" @click="deleteMovie(movie)">Supprimer</button>
        </li>
    </ul>

    <button class="button" @click="sortMovies">Réorganiser</button>
    <br>
    <form @submit.prevent="addMovie">
        <!-- Le .prevent pour preventDefault() -->

        <!-- v-model est l'équivalent de onChange en React mais un peu plus spécifique
        au éléments de formulaire. Il permet de lier une référence à la valeur d'un élément de formulaire.
        -->
        <label for="newMovie">Nouveau film</label>
        <input id="newMovie" type="text" name="newMovie" placeholder="Nouveau film" v-model="newMovie">
        <button class="button">Ajouter</button>
    </form>
</template>







<script lang="ts" setup>
import {ref} from 'vue';

//  L'équivalent des states est la ref
const count = ref(0);

// On change un state grace à l'attribut value
const increment = (event) => {
    console.log(event);
    count.value++;
}

const decrement = () => {
    count.value--;
}





const movies = ref([
    "Matrix",
    "Le Roi Lion",
    "Titanic"
])

const deleteMovie = (movie) => {
    movies.value = movies.value.filter(m => m != movie)
}

const sortMovies = () => {
    movies.value.sort();
}

const newMovie = ref('');

const addMovie = () => {
    movies.value.push(newMovie.value);
    newMovie.value = '';
}
</script>





<style>
body {
    padding: 250px;
}

.buttons {
    display: flex;
    align-items: center;
    justify-content: space-around;
}

.buttons button {
    padding: 10px 20px;
    border: 1px solid black;
    border-radius: 7px;

    cursor: pointer;
}

.button {
    margin: 30px;
    padding: 10px 20px;
}

.counter {
    text-align: center;
}

.message {
    text-align: center;
}

.green {
    color: green;
}

.red {
    color: red;
}
</style>