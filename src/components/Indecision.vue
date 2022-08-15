<template>
    <img v-if="img" :src="img" alt="bg">
    <div class="bg-dark"></div>  
    <div class="indecision-container">
        <input
          v-model="question"
          type="text"
          placeholder="Ask me a question">
        <p>Remember Finished with (?)</p>
        <div v-if="isValidationQuestion">
            <h2>{{question}}</h2>
            <h1>{{answer}}</h1>
        </div>
    </div>
</template>

<script>
export default {
    data() {
        return {
            question:"",
            answer: null,
            img:null,
            isValidationQuestion: null
        }
    },
    methods: {
        async getanswer() {
            this.answer = "pensando...";
            const {answer,image} = await fetch("https://yesno.wtf/api").then( r => r.json()); 
            this.answer = answer;
            this.img = image;
        }
    },
    watch: {
        question(value,oldValue) {
            this.isValidationQuestion = false;
            if(!value.includes("?")) return;
            this.isValidationQuestion = true;
            this.getanswer()
        }
    }

}
</script>

<style scoped>
    img, .bg-dark {
        height: 100vh;
        left: 0px;
        max-height: 100%;
        max-width: 100%;
        position: fixed;
        top: 0px;
        width: 100vw;
        object-fit: cover;
        object-position: center center;
    }

    .bg-dark {
        background-color: rgba(0, 0, 0, 0.4);
    }

    .indecision-container {
        position: relative;
        z-index: 99;
    }
    input {
        width: 250px;
        padding: 10px 15px;
        border-radius: 5px;
        border: none;
        margin-bottom: 20px;
    }
    input:focus {
        outline: none;
    }
    p {
        color: white;
        font-size: 18px;
        margin-top: 0px;
    }
    h1, h2 {
        color: white;
    }
    h2 {
        font-size: 35px;
        margin-top: 150px;
    }
</style>