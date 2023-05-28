<template>
<div class="generator-container">
    <h1 class="title">מחשבון המחאות</h1>
    <div class="header-container">
        <div class="input-holder">
            <input v-model="checksAmount" id="check-amount" type="number">
            <label for="check-amount">מספר צ'קים רצוי</label>
        </div>
        <div class="input-holder">
            <input v-model="price" id="price" type="number">
            <label for="check-amount">סה"כ לתשלום</label>
        </div>
        <div class="btn-holder">
            <button @click="generateChecks()">
                <h1>הפעל</h1>
            </button>
        </div>
    </div>
    <div v-if="answer" class="output-container">
        <h1 dir="rtl">{{ answer }}</h1>
    </div>
</div>
</template>
<script>
export default{
  props:{

  },
  emits:[],
  data() {
    return{
        answer: null,
        checksAmount: 0,
        price:0,
    }
},
methods:{
    generateChecks(){
        let paymentPerCheck = this.price/this.checksAmount
        if (this.price%this.checksAmount == 0){
            this.answer = 'יש להכין ' + this.checksAmount + ' המחאות, כל אחת על סך ' + paymentPerCheck + ' שקלים ' 
        }else{
            let firstPayment = Math.floor(paymentPerCheck)
            let samePriceChecksAmount = this.checksAmount - 1
            let lastPayment = this.price - (firstPayment * samePriceChecksAmount)
            this.answer = 'יש להכין ' + samePriceChecksAmount + ' המחאות על סך ' + firstPayment + ' שקלים והמחאה 1 על סך ' + lastPayment + ' שקלים'
        }
    }
}
}
</script>
