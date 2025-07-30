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

        <transition-group name="bounce" tag="ul" id="list">
            <li :key="list.indexOf(prop)" v-for="prop in list" class="proposition">
                <p> {{ prop }} </p>
                <button class="close" @click="deleteElementList(prop)">
                    <svg xmlns="http://www.w3.org/2000/svg" viewBox="0 0 320 512"><!--! Font Awesome Pro 6.1.1 by @fontawesome - https://fontawesome.com License - https://fontawesome.com/license (Commercial License) Copyright 2022 Fonticons, Inc. --><path d="M310.6 361.4c12.5 12.5 12.5 32.75 0 45.25C304.4 412.9 296.2 416 288 416s-16.38-3.125-22.62-9.375L160 301.3L54.63 406.6C48.38 412.9 40.19 416 32 416S15.63 412.9 9.375 406.6c-12.5-12.5-12.5-32.75 0-45.25l105.4-105.4L9.375 150.6c-12.5-12.5-12.5-32.75 0-45.25s32.75-12.5 45.25 0L160 210.8l105.4-105.4c12.5-12.5 32.75-12.5 45.25 0s12.5 32.75 0 45.25l-105.4 105.4L310.6 361.4z"/></svg>
                </button>
            </li>
        </transition-group>

        <button class="reset_list" v-if="list.length > 1" @click="resetList()">Clear list</button>

        <transition name="slide-fade">
            <form @submit.prevent  class="load_saver" v-if="saver">
                <input class="input_saver" v-model="name_list" name="input_saver" type="text" placeholder="Name of list...">
                <button id="save" @click="saveList()">Save</button>
            </form>
        </transition>

        <transition name="slide-fade">
            <ul id="own_list" v-if="shower">
                <li :key="list_saved.indexOf(prop)" v-for="prop in list_saved" class="a_list">
                    <p class="name_list" @click="showThisList(prop)"> {{ prop }} </p>
                    <button @click="deleteThisList(prop)">
                        <svg xmlns="http://www.w3.org/2000/svg" viewBox="0 0 448 512"><!--! Font Awesome Pro 6.2.1 by @fontawesome - https://fontawesome.com License - https://fontawesome.com/license (Commercial License) Copyright 2022 Fonticons, Inc. --><path d="M135.2 17.7C140.6 6.8 151.7 0 163.8 0H284.2c12.1 0 23.2 6.8 28.6 17.7L320 32h96c17.7 0 32 14.3 32 32s-14.3 32-32 32H32C14.3 96 0 81.7 0 64S14.3 32 32 32h96l7.2-14.3zM32 128H416V448c0 35.3-28.7 64-64 64H96c-35.3 0-64-28.7-64-64V128zm96 64c-8.8 0-16 7.2-16 16V432c0 8.8 7.2 16 16 16s16-7.2 16-16V208c0-8.8-7.2-16-16-16zm96 0c-8.8 0-16 7.2-16 16V432c0 8.8 7.2 16 16 16s16-7.2 16-16V208c0-8.8-7.2-16-16-16zm96 0c-8.8 0-16 7.2-16 16V432c0 8.8 7.2 16 16 16s16-7.2 16-16V208c0-8.8-7.2-16-16-16z"/></svg>
                    </button>
                </li>
            </ul>
        </transition>

        <transition name="slide-fade">
            <div class="load_saver" v-if="saver">
                <button v-if="!shower" id="load" @click="loadOwnList()">Show your lists</button>
                <button v-if="shower" id="hide" @click="loadOwnList()">Hide your lists</button>
            </div>
        </transition>
    </main>

    <footer>
        <p><button @click="activeSaver()" class="coded">Coded</button> by <a target="_blank" href="https://www.linkedin.com/in/fabio-ramoslopes/">Fabio R. LOPES</a></p>
    </footer>
</template>


<script>
    export default {
        name: 'RandomChoicer',
        data() {
            return {
                inputValue: '',
                list: [],
                saver: false,
                name_list: '',
                list_saved: [],
                shower: false,
            }
        },
        mounted() {
            if(!localStorage.getItem('rc_list_saved')) {
                localStorage.setItem('rc_list_saved', JSON.stringify(this.list_saved));
            } else {
                this.list_saved = JSON.parse(localStorage.getItem('rc_list_saved'));
            }
        },
        methods: {
            activeSaver() {
                this.saver = !this.saver;

                if(this.shower === true) {
                    this.shower = !this.shower;
                };

                const btn_coded = document.querySelector('.coded');
                btn_coded.classList.toggle('bgsaver');
            },
            addToList() {
                if(this.inputValue.length === 0 || this.list.includes(this.inputValue.trim())) {
                    this.inputValue = '';
                    return;
                }

                const list = document.querySelectorAll('.proposition');
                list.forEach(el => {
                    el.classList.remove('active');
                });

                this.list.push(this.inputValue.trim());
                this.inputValue = '';
            },
            deleteElementList(prop) {
                const indexToDel = this.list.indexOf(prop);
                const list = document.querySelectorAll('.proposition');
                list.forEach(el => {
                    el.classList.remove('active');
                });

                const deleted = document.querySelectorAll('.proposition')[indexToDel]
                deleted.classList.add('delete-item');

                setTimeout(() => {
                    this.list.splice(indexToDel, 1);
                    list.forEach(el => {
                    el.classList.remove('delete-item');
                })
                }, 500);

            },
            resetList() {
                const list = document.querySelectorAll('.proposition');
                list.forEach(el => {
                    el.classList.add('delete-item');
                })
                setTimeout(() => {
                    this.list = [];
                }, 450);
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

                for(let i = 0; i < list.length; i++) {
                    if(i % 2 == 0) { list[i].classList.add('animate')}
                    else { list[i].classList.add('animate2')}
                }

                setTimeout(() => {
                    list.forEach(el => {
                        el.classList.remove('animate');
                        el.classList.remove('animate2');
                    })

                    const choice = document.querySelectorAll('.proposition')[indexSelected];
                    choice.classList.add('active');
                    choice.scrollIntoView({
                        behavior: "smooth",
                        block: "center",
                        inline: "center"
                    });
                }, "2550");

            },
            saveList() {
                if(!this.list_saved.includes(this.name_list)) {
                    this.list_saved.push(this.name_list);
                    localStorage.setItem('rc_list_saved', JSON.stringify(this.list_saved));
                }
                localStorage.setItem(this.name_list, JSON.stringify(this.list));
                this.name_list = '';

                Toastify({
                    text: "Your list has been saved",
                    duration: 4000,
                    newWindow: true,
                    gravity: "bottom", // `top` or `bottom`
                    position: "center", // `left`, `center` or `right`
                    stopOnFocus: true, // Prevents dismissing of toast on hover
                    style: {
                        background: "#329267",
                        color: "#222",
                    }
                }).showToast();
            },
            loadOwnList() {
                if(this.list_saved.length > 0) {
                    this.shower = !this.shower;
                }
            },
            showThisList(name_list) {
                if(localStorage.getItem(name_list)) {
                    this.list = JSON.parse(localStorage.getItem(name_list));
                }
                this.shower = !this.shower;
                this.saver = !this.saver;
                const btn_coded = document.querySelector('.coded');
                btn_coded.classList.toggle('bgsaver');
            },
            deleteThisList(name_list) {
                const response = window.confirm(`Are you sure do you want to delete the list : ${name_list}`);

                if(!response) {
                    return;
                }

                if(localStorage.getItem('rc_list_saved')) {
                    const updatedList = JSON.parse(localStorage.getItem('rc_list_saved'));
                    updatedList.splice(updatedList.indexOf(name_list), 1);
                    localStorage.setItem('rc_list_saved', JSON.stringify(updatedList));
                    localStorage.removeItem(name_list);
                    this.list_saved = JSON.parse(localStorage.getItem('rc_list_saved'));
                }

                if(this.list_saved.length < 1) {
                    this.shower = false;
                }

                Toastify({
                    text: "Your list has been deleted",
                    duration: 4000,
                    newWindow: true,
                    gravity: "bottom", // `top` or `bottom`
                    position: "center", // `left`, `center` or `right`
                    stopOnFocus: true, // Prevents dismissing of toast on hover
                    style: {
                        background: "#329267",
                        color: "#222",
                    }
                }).showToast();
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

    html, body {
        overscroll-behavior: none;
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
        height: 50px;
        width: 310px;
        border-radius: 50px;
        display: flex;
        align-items: center;
        margin-bottom: 25px;
    }

    #input_box input {
        font-family: 'Righteous', cursive, Helvetica, sans-serif;
        background-color: #F3F3F3;
        font-size: 1em;
        height: 100%;
        width: 310px;
        border-radius: 50px;
        outline: none;
        padding-left: 20px;
        padding-right: 85px;
    }

    #input_box input:focus {
        box-shadow: 0 0 8px 3px rgba(65, 184, 131, .7);
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

    input.input_saver {
        background-color: whitesmoke;
        /* margin: 25px 0 12px 0 */
        width: 100%;
        height: 50px;
        border-radius: 50px;
        overflow: hidden;
        font-size: 1em;
        outline: none;
        text-align: left;
        padding-left: 20px;
        padding-right: 102px;
        font-family: 'Righteous', cursive, Helvetica, sans-serif;
        font-style: italic;
        font-size: 1em;
    }

    input.input_saver:focus {
        box-shadow: 0 0 8px 3px rgba(34, 116, 165, .7);
        border: 1px solid #2274A5;
    }

    .load_saver {
        margin: 12px 0 0 0;
        position: relative;
        display: flex;
        justify-content: space-between;
        width: 100%;
        height: 50px;
        border-radius: 50px;
    }

    #save {
        position: absolute;
        right: 0;
        cursor: pointer;
        font-family: 'Righteous', cursive, Helvetica, sans-serif;
        letter-spacing: .5px;
        font-size: 1em;
        width: 30%;
        height: 100%;
        border-radius: 50px;
        background-color: #2274A5;
        color: white;
    }

    #load {
        cursor: pointer;
        font-family: 'Righteous', cursive, Helvetica, sans-serif;
        letter-spacing: .5px;
        font-size: 1em;
        width: 100%;
        height: 100%;
        border-radius: 50px;
        background-color: #2274A5;
        color: white;
    }

    #hide {
        cursor: pointer;
        font-family: 'Righteous', cursive, Helvetica, sans-serif;
        letter-spacing: .5px;
        font-size: 1em;
        width: 100%;
        height: 100%;
        border-radius: 50px;
        background-color: #18374c;
        color: whitesmoke;
    }

    #delete {
        display: flex;
        justify-content: center;
        align-items: center;
        cursor: pointer;
        font-family: 'Righteous', cursive, Helvetica, sans-serif;
        letter-spacing: .5px;
        font-size: 1em;
        width: 30%;
        height: 100%;
        border-radius: 50px;
        background-color: crimson;
        color: white;
    }

    #delete svg {
        height: 42%;
        fill: white;
    }

    .load_saver button:active {
        transform: scale(.92);
    }

    .load_saver select {
        font-family: 'Righteous', cursive, Helvetica, sans-serif;
        background-color: whitesmoke;
        color: #000;
        padding: 0 20px;
        width: 100%;
        height: 100%;
        border-radius: 50px;
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

    #own_list {
        margin: 20px 0 0 0;
    }

    .a_list {
        position: relative;
        cursor: pointer;
        background-color: #34495e65;
        height: 50px;
        width: 310px;
        border-radius: 50px;
        display: flex;
        justify-content: space-between;
        align-items: center;
        padding: 0 6px 0 22px;
        margin-bottom: 2px;
    }

    .name_list {
        display: flex;
        justify-content: flex-start;
        align-items: center;
        height: 100%;
        width: calc(100% - 60px);
        overflow: hidden;
        padding-left: 10px;
        letter-spacing: 1px;
    }

    .name_list:active {
        transform: scale(.96);
    }

    .a_list button {
        cursor: pointer;
        height: 100%;
        width: 50px;
        display: flex;
        justify-content: center;
        align-items: center;
        border-radius: 50%;
        background-color: transparent;
    }

    .a_list button svg {
        fill: #F3F3F3;
        height: 40%;
        z-index: 5;
    }

    .a_list button:active {
        transform: scale(.85);
    }

    .a_list button:active svg{
        fill: crimson;
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
        font-size: 1.1em;
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

    .coded {
        cursor: pointer;
        font-family: 'Righteous', cursive, Helvetica, sans-serif;
        font-size: 1em;
        background-color: #41B883;
        color: #181818;
        border-radius: 50px;
        padding: 2px 8px;
        margin: 0 2px;
    }

    .bgsaver {
        background-color: #2274A5;
        color: white;
    }

    .coded:active {
        transform: scale(.92);
    }

    .active {
        transform: scale(1.1);
        background-color: #41B883;
        z-index: 10;
    }

    .bg_tomato {
        background-color: tomato;
    }

    .animate {
        animation: animChoicer 2.5s ease-in-out backwards;
    }

    .animate2 {
        animation: animChoicer2 2.5s ease-in-out backwards;
    }

    .bounce-enter-active {
        animation: bounce-in 500ms;
    }

    @keyframes bounce-in {
        0% {
            transform: scale(0);
        }
        50% {
            transform: scale(1.1);
        }
        100% {
            transform: scale(1);
        }
    }

    .delete-item {
        animation: delete-out 500ms forwards;
    }

    @keyframes delete-out {
        0% {
            transform: scale(1);
        }
        50% {
            transform: scale(1.1);
        }
        100% {
            transform: scale(0);
            display: none;
        }
    }

    .slide-fade-enter-active {
        transition: all 400ms ease-out;
    }

    .slide-fade-leave-active {
        transition: all 400ms ease-in;
    }

    .slide-fade-enter-from,
    .slide-fade-leave-to {
        transform: translateY(20px);
        opacity: 0;
    }

    @keyframes animChoicer {
        0% {
            transform: scale(.95);
        }
        12.5% {
            transform: scale(1.05);
        }
        25% {
            transform: scale(.95);
        }
        37.5% {
            transform: scale(1.05);
        }
        50% {
            transform: scale(.95);
        }
        62.5% {
            transform: scale(1.05);
        }
        75% {
            transform: scale(.95);
        }
        87.5% {
            transform: scale(1.05);
        }
        100% {
            transform: scale(.95);
        }
    }

    @keyframes animChoicer2 {
        0% {
            transform: scale(1.05);
        }
        12.5% {
            transform: scale(.95);
        }
        25% {
            transform: scale(1.05);
        }
        37.5% {
            transform: scale(.95);
        }
        50% {
            transform: scale(1.05);
        }
        62.5% {
            transform: scale(.95);
        }
        75% {
            transform: scale(1.05);
        }
        87.5% {
            transform: scale(.95);
        }
        100% {
            transform: scale(.925);
        }
    }
</style>