<template>
    <div>
        
        <div id="screen" v-bind:class="state" @click="onClickScreen"> {{ message }} </div>
       
        <template v-if="result.length">     <!-- v-show -->
            <div>
                <div> Average time : {{ average }}ms </div>
                <button @click="onReset"> Reset </button>
            </div>
        </template>

    </div>
</template>

<script>

    let startTime = 0
    let endTime = 0
    let timeout = null

    export default {
        data() {
            return {
                result: [],
                state: 'waiting',
                message: 'Click to start'
            }
        },
        computed: {         // 일반 데이터를 가공해서 사용할 경우, computed 사용. 값이 caching 된다. (화면 전환시 연산되는 내용 줄일 수 있음)
            average() {
                return this.result.reduce((a,c) => a+c , 0) / this.result.length || 0
            }
        },
        methods: {
            onReset() {
                this.result = []
            },
            onClickScreen() {
                if (this.state === 'waiting') {
                    this.state = 'ready'
                    this.message = 'Click when the background color turn to green '

                    setTimeout(() => {
                        this.state = "now"
                        this.message = "Click now"
                        startTime = new Date()
                    }, Math.floor(Math.random() * 1000) + 2000)

                } else if (this.state === 'ready')  {
                    this.state = 'waiting'
                    this.message = 'Too hasty!'

                    clearTimeout(timeout)
                } else if (this.state === 'now') {
                    endTime = new Date()
                    this.state = 'waiting'
                    this.message = 'Click to start'

                    this.result.push(endTime - startTime)
                }
            },
        }        
    }
</script>

<style scoped>

    #screen {
        width: 300px;
        height: 200px;
        text-align: center;
        user-select: none;
    }
    
    #screen.waiting {
        background-color: aqua;
    }
    
    #screen.ready {
        background-color: red;
    }

    #screen.now {
        background-color: greenyellow;
    }

</style>