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
    <footer>
        <h1>By Shiran Abir</h1>
        <h2 dir="rtl">יש תקלה? יש לכם רעיון לתוכנה נוספת?</h2>
        <h2 dir="rtl">דברו איתי - shirabir@openu.ac.il / shirkiabir@gmail.com</h2>
    </footer>
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
        checksAmount: null,
        price:null,
    }
},
methods:{
    generateChecks(){
        if (this.checksAmount <= 0||this.price <= 0) return
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
