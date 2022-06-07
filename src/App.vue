<template>
    <header>
        <svg xmlns="http://www.w3.org/2000/svg" viewBox="0 0 512 512"><!-- ! Font Awesome Pro 6.1.1 by @fontawesome - https://fontawesome.com License - https://fontawesome.com/license (Commercial License) Copyright 2022 Fonticons, Inc. --><path d="M424.1 287c-15.13-15.12-40.1-4.426-40.1 16.97V352H336L153.6 108.8C147.6 100.8 138.1 96 128 96H32C14.31 96 0 110.3 0 128s14.31 32 32 32h80l182.4 243.2C300.4 411.3 309.9 416 320 416h63.97v47.94c0 21.39 25.86 32.12 40.99 17l79.1-79.98c9.387-9.387 9.387-24.59 0-33.97L424.1 287zM336 160h47.97v48.03c0 21.39 25.87 32.09 40.1 16.97l79.1-79.98c9.387-9.391 9.385-24.59-.0013-33.97l-79.1-79.98c-15.13-15.12-40.99-4.391-40.99 17V96H320c-10.06 0-19.56 4.75-25.59 12.81L254 162.7L293.1 216L336 160zM112 352H32c-17.69 0-32 14.31-32 32s14.31 32 32 32h96c10.06 0 19.56-4.75 25.59-12.81l40.4-53.87L154 296L112 352z"/></svg>
        <h1>RANDOM <br> CHOICER</h1>
    </header>

    <main>
        <button id="random_choice" @click="randomChoice()">Let's Go</button>

        <form @submit.prevent id="input_box">
            <input v-model="inputValue" type="text" placeholder="Write elements of list...">
            <button @click="addToList()">ADD</button>
        </form>

        <ul id="list">
            <li :key="list.indexOf(prop)" v-for="prop in list" class="proposition">
                <p> {{ prop }} </p>
                <button class="close" @click="deleteElementList(prop)">
                    <svg xmlns="http://www.w3.org/2000/svg" viewBox="0 0 320 512"><!--! Font Awesome Pro 6.1.1 by @fontawesome - https://fontawesome.com License - https://fontawesome.com/license (Commercial License) Copyright 2022 Fonticons, Inc. --><path d="M310.6 361.4c12.5 12.5 12.5 32.75 0 45.25C304.4 412.9 296.2 416 288 416s-16.38-3.125-22.62-9.375L160 301.3L54.63 406.6C48.38 412.9 40.19 416 32 416S15.63 412.9 9.375 406.6c-12.5-12.5-12.5-32.75 0-45.25l105.4-105.4L9.375 150.6c-12.5-12.5-12.5-32.75 0-45.25s32.75-12.5 45.25 0L160 210.8l105.4-105.4c12.5-12.5 32.75-12.5 45.25 0s12.5 32.75 0 45.25l-105.4 105.4L310.6 361.4z"/></svg>
                </button>
            </li>
        </ul>

        <div v-if="list.length > 1">
            <button class="reset_list" @click="resetList()">
                Clear list
            </button>
        </div>
    </main>

    <footer>
        <p>Coded by <a target="_blank" href="https://www.linkedin.com/in/fabio-ramoslopes/">Fabio R. LOPES</a></p>
    </footer>
</template>


<script>
    export default {
        name: 'RandomChoicer',
        data() {
            return {
                inputValue: '',
                list: [],

            }
        },
        methods: {
            addToList() {
                if(this.inputValue.length === 0 || this.list.includes(this.inputValue)) {
                    this.inputValue = '';
                    return;
                }

                const list = document.querySelectorAll('.proposition');
                list.forEach(el => {
                    el.classList.remove('active');
                });

                this.list.push(this.inputValue);
                this.inputValue = '';
            },

            deleteElementList(prop) {
                this.list.splice(this.list.indexOf(prop), 1);

                const list = document.querySelectorAll('.proposition');
                list.forEach(el => {
                    el.classList.remove('active');
                });
            },

            resetList() {
                this.list = [];
            },

            randomChoice() {
                if(this.list.length <= 1) {
                    return;
                }

                const indexSelected = Math.floor(Math.random() * ((this.list.length) - 0) + 0);

                const list = document.querySelectorAll('.proposition');
                list.forEach(el => {
                    el.classList.remove('active');
                });

                const choice = document.querySelectorAll('.proposition')[indexSelected];
                choice.classList.add('active');
            }
        },
    }
</script>


<style>
    /*------RESET------*/
    *, ::before, ::after {
        box-sizing: border-box;
        padding: 0;
        margin: 0;
    }

    body {
        overflow-x: hidden;
        min-width: 375px;
        background-color: #181818;
        color: #F3F3F3;
        font-family: 'Righteous', cursive, Helvetica, sans-serif;
    }

    a {
        text-decoration: none;
        color: #F3F3F3;
    }

    a:hover {
        color: #41B883;
    }

    button, input {
        border: none;
    }

    * {
    scrollbar-width: thin;
    scrollbar-color: rgb(18, 18, 18) rgb(243, 243, 243, .2);
    }

    *::-webkit-scrollbar {
        width: 8px;
        height: 10px;
    }

    ::-webkit-scrollbar-track {
        background: rgba(243, 243, 243, .2);
    }

    ::-webkit-scrollbar-button {
        width: 0px;
        height: 0px;
    }
    ::-webkit-scrollbar-thumb {
        background: #41B883;
    }

    ::selection {
        background-color: #41B883;
        color: white;
    }

    /*------HEADER------*/
    header {
        display: flex;
        justify-content: center;
        width: 375px;
        margin: 25px auto;
    }

    header h1 {
        font-family: 'Righteous', cursive, Helvetica, sans-serif;
        color: white;
        font-size: 2.5em;
        width: max-content;
    }

    header svg {
        height: 100px;
        width: 100px;
        fill: #41B883;
        margin-right: 12px;
    }

    /*------MAIN------*/
    main {
        width: max-content;
        height: max-content;
        display: flex;
        flex-direction: column;
        align-items: center;
        margin: 0 auto;
    }

    #random_choice {
        cursor: pointer;
        background-color: #41B883;
        color: #F3F3F3;
        border: none;
        font-family: 'Righteous', cursive, Helvetica, sans-serif;
        font-size: 1.5em;
        height: 50px;
        width: 310px;
        border-radius: 50px;
        margin-bottom: 25px;
    }

    #random_choice:active {
        transform: scale(.95);
    }

    #input_box {
        position: relative;
        background-color: #F3F3F3;
        background-color: tomato;
        height: 50px;
        width: 310px;
        border-radius: 50px;
        display: flex;
        align-items: center;
        margin-bottom: 25px;
    }

    #input_box input {
        font-family: 'Righteous', cursive, Helvetica, sans-serif;
        font-size: 1em;
        height: 100%;
        width: 310px;
        border-radius: 50px;
        outline: none;
        padding-left: 20px;
        padding-right: 85px;
    }

    #input_box input:focus {
        box-shadow: 0 0 6px 2px rgba(65, 184, 131, .7);
        border: 1px solid #41B883;
    }

    #input_box button {
        position: absolute;
        cursor: pointer;
        right: 0px;
        background-color: #41B883;
        color: #F3F3F3;
        font-family: 'Righteous', cursive, Helvetica, sans-serif;
        font-size: 1.4em;
        height: 100%;
        width: 75px;
        border-radius: 50px;
    }

    #input_box button:active {
        transform: scale(.92);
    }

    #list {
        width: 310px;
        height: max-content;
    }

    .proposition {
        position: relative;
        background-color: #34495E;
        height: 50px;
        width: 310px;
        border-radius: 50px;
        display: flex;
        justify-content: space-between;
        align-items: center;
        padding: 0 6px 0 22px;
        margin-bottom: 2px;
    }

    .proposition button {
        cursor: pointer;
        height: 100%;
        width: 50px;
        display: flex;
        justify-content: center;
        align-items: center;
        border-radius: 50%;
        background-color: transparent;
    }

    .proposition button:active {
        transform: scale(.90);
    }

    .proposition button svg {
        fill: #F3F3F3;
        height: 60%;
        z-index: 5;
    }

    .reset_list {
        display: flex;
        justify-content: center;
        align-items: center;
        cursor: pointer;
        margin: 25px 0;
        height: 50px;
        width: 155px;
        border-radius: 50px;
        font-family: 'Righteous', cursive, Helvetica, sans-serif;
        font-size: 1.2em;
        background-color: crimson;
        color: #F3F3F3;
    }

    .reset_list:active {
        transform: scale(.95);
    }


    /*------FOOTER------*/

    footer {
        height: 100px;
        width: 310px;
        margin: 25px auto 0 auto;
        display: flex;
        justify-content: center;
        align-items: center;
    }

    .active {
        transform: scale(1.1);
        background-color: #41B883;
        z-index: 10;
    }

</style>
