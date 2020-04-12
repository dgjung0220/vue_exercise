<template>
    <div>
        <h1> {{ result }} </h1>
        <form @submit.prevent="onSubmitForm">
            <input ref="answer" maxlength="4" v-model="value"/>
            <button type="submit"> Go </button>
        </form>
        <div> try : {{ tries_array.length }}</div>

        <ul>
            <li v-for="t in tries_array">
                <div>{{ t.value }}</div>
                <div>{{ t.result }}</div>    
            </li>
        </ul>
    </div>
</template>

<script>

    const getNumbers = () => {
        const candidates = [1, 2, 3, 4, 5, 6, 7, 8, 9]
        const array = []

        for (let i = 0; i < 4; i++) {
            var chosen = candidates.splice(Math.floor(Math.random() * (9-i)), 1)[0]
            array.push(chosen)
        }
        return array
    }

    export default {
        data() {
            return {
                answer: getNumbers(),
                tries_array: [],
                value: '',
                result: ''
            }
        },
        methods: {
            onSubmitForm() {
                
                console.log(this.answer)

                if (this.value === this.answer.join('')) {
                    
                    this.tries_array.push({
                        value : this.value,
                        result: 'Home run'
                    })

                    this.result = 'Home run'
                    alert('Retry new Game')
                    this.value = ''
                    this.tries_array = [];

                } else {

                    if (this.tries_array.length >= 9) {
                        this.result = `Try until 10 times. The answer is ${this.answer.join(',')}`
                        alert('Retry new game')
                        this.value = ''
                        this.answer = getNumbers()
                        this.tries_array = []
                        this.$refs.answer.focus()
                    }

                    let strike = 0
                    let ball = 0 
                    const answerArray = this.value.split('').map(v => parseInt(v));

                    for (let i = 0; i < 4; i++) {
                        if (answerArray[i] === this.answer[i]) {
                            strike++   
                        } else if (this.answer.includes(answerArray[i])){
                            ball++
                        }
                    }

                    this.tries_array.push({
                        value : this.value,
                        result: `${strike} Strike, ${ball} Ball`
                    })
                }

                this.value = ''
                this.$refs.answer.focus();
            }
        }        
    }
</script>

<style>
</style>