<template>
    <div id="app">
        <div>
            {{question}}
        </div>
        <div>
            <input type="text" ref="input" autofocus @keyup.enter="handlerClick">
            <button type="button" @click="handlerClick">입력</button>
        </div>
        <template v-if="notiArray.length > 0">
            <div v-for="(noti, i) in notiArray" :key="i">
                {{noti.answer}} : {{noti.question}} = {{noti.userNumber}}
            </div>
        </template>
    </div>
</template>

<script>
export default {
    name: 'app',
    data(){
        return{
            answer : '',

            firstNumber : 2, // 2단부터 시작
            SecondNumber : 0, // 1부터 시작
            
            userNumber : '',
            notiArray : [],
        }
    },
    created(){
        this.Ready();
    },

    computed : {
        question(){
           return '문제 : ' + this.firstNumber + ' X ' + this.SecondNumber + ' ?';
        }
    },

    methods : {
        Ready(){
            this.SecondNumber ++;
            
            if(this.SecondNumber % 10 === 0){
                this.firstNumber ++;
                this.SecondNumber = 1;
                
                if(this.firstNumber === 10){
                    this.firstNumber = 2;
                }
            }
        },

        handlerClick(){
            const target = this.$refs.input;
            const targetVal = target.value;
            
            if(targetVal !== ''){
                this.userNumber = Number(targetVal);
                this.result();

                target.value = '';
            }
        },

        result(){
            this.answer = this.firstNumber * this.SecondNumber;
            let resultMgs = (this.answer === this.userNumber) 
                ? '정답' 
                : '오답';

            this.notiArray.push({ 
                question : this.firstNumber + ' X ' + this.SecondNumber,
                userNumber : this.userNumber,
                answer : resultMgs
            });

            if(this.answer === this.userNumber) {
                this.Ready();
            }
        }
    }
}
</script>

<style>

</style>
