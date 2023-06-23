<template>
<div class="generator-container">
    <h1 class="title">מחשבון המחאות</h1>
    <div class="header-container">
        <h1 class="section_title">פרטי התשלום</h1>
        <div class="input-holder">
            <input v-model="info.checksAmount" id="check-amount" type="number">
            <label for="check-amount">מספר צ'קים רצוי</label>
        </div>
        <div class="input-holder">
            <input v-model="info.price" id="price" type="number">
            <label for="check-amount">סה"כ לתשלום</label>
        </div>
        <h1 class="section_title">פרטי הקורס</h1>
        <div class="input-holder">
            <select v-model="info.isStarted" id="isStarted">
                <option :value=true>כן</option>
                <option :value=false>לא</option>
            </select>
            <label for="check-amount">?הקורס התחיל</label>
        </div>
        <div v-show="!info.isStarted" class="input-holder">
            <input v-model="info.startDate" type="date">
            <label for="check-amount">תאריך פתיחה</label>
        </div>
        <div class="input-holder">
            <input v-model="info.endDate" type="date">
            <label for="check-amount">תאריך סיום</label>
        </div>
        <div class="btn-holder">
            <button @click="generateChecks()">
                <h1>הפעל</h1>
            </button>
        </div>
    </div>
    <div v-if="answer" class="output-container">
        <textarea dir="rtl" :value="answer" name="" id="" cols="30" rows="10"></textarea>
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
        info:{
            checksAmount: null,
            price:null,
            isStarted:false,
            startDate:null,
            endDate:null
        },
        
    }
},
methods:{
    generateChecks(){
        let {checksAmount,price,endDate,startDate} = this.info
        if(!checksAmount ||!price || !endDate) return alert("יש למלא את כל הפרטים!")
        const start = new Date(startDate);
        const end = new Date(endDate);
        let paymentPerCheck = price/checksAmount
        const isEvenPays = price%checksAmount == 0
        let firstPayment
        let samePriceChecksAmount
        let lastPayment
        if (!isEvenPays){
            firstPayment = Math.floor(paymentPerCheck)
            samePriceChecksAmount = checksAmount - 1
            lastPayment = price - (firstPayment * samePriceChecksAmount)
            if (!startDate) {
            let dates = this.getAllMonths(new Date(),end)
            let lastPayDate = ''
            lastPayDate += dates.pop()
            lastPayDate += ` - ${lastPayment} ש"ח\n`
            let payDate = ''
            dates.forEach(date => payDate += `${date} - ${firstPayment} ש"ח\n`)
            
            this.answer = `תלמיד/ה יקר/ה,

סה"כ לתשלום: ${price} ש"ח.

פירוט הצ'קים:

${payDate}${lastPayDate}`

        }else{
            let dates = this.getAllMonths(start,end)
            let lastPayDate = ''
            lastPayDate += dates.pop()
            lastPayDate += ` - ${lastPayment} ש"ח\n`
            let payDate = ''
            dates.forEach(date => payDate += `${date} - ${firstPayment} ש"ח\n`)
            
            this.answer = `תלמיד/ה יקר/ה,

סה"כ לתשלום: ${price} ש"ח.

פירוט הצ'קים:

${payDate}${lastPayDate}`
        } 
            
        }else{
            if (!startDate) {
            let dates = this.getAllMonths(new Date(),end)
            let payDate = ''
            dates.forEach(date => payDate += `${date} - ${paymentPerCheck} ש"ח\n`)
            this.answer = `תלמיד/ה יקר/ה,

סה"כ לתשלום: ${price} ש"ח.

פירוט הצ'קים:

${payDate}`
        }else{
            let dates = this.getAllMonths(start,end)
            let payDate = ''
            dates.forEach(date => payDate += `${date} - ${paymentPerCheck} ש"ח\n`)
            this.answer = `תלמיד/ה יקר/ה,

סה"כ לתשלום: ${price} ש"ח.

פירוט הצ'קים:

${payDate}`
        } 
        }
               
    },
    resetInfo(){
        this.info = {
            checksAmount: null,
            price:null,
            isStarted:false
        }
    },
    getAllMonths(startDate, endDate) {
        let months = [];
        
        const currentDate = new Date(startDate.getTime());
        while (currentDate <= endDate) {
            const year = currentDate.getFullYear();
            const month = currentDate.getMonth();
 
            months.push({ year, month });
            currentDate.setMonth(month + 1);
        }
        months.pop()
        months = months.slice(0, parseInt(this.info.checksAmount))
        if (this.info.checksAmount>months.length) return alert(`כמות הצ'קים המקסימלית היא ${months.length} צ'קים!`)
        return this.formatDates(months);
    },
    formatDates(dates){
        let payDates = []
        dates.forEach(date => payDates.push(`${date.month+1}.${date.year}`))
        return payDates
    }

},
watch:{
    info: {
        handler(newValue){
            console.log('here');
            if (newValue.isStarted === true){
                this.info.startDate = null
            }
        },
        deep: true
    }
}
}
</script>
