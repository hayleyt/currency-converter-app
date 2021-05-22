<template>
   <div class="page">
      <h1>{{fromCurrency.name}} to {{toCurrency.name}}</h1>
      <h2>Convert {{fromCurrency.code}} to {{toCurrency.code}} at today's exchange rate.</h2>
      <div class="converter">
         <div class="area1">
            <label for="amount" class="label">Amount:</label>
            <input type="number" inputmode="decimal" tabindex="0" id="amount" name="amount" v-model="fromAmount" @keyup="toAmount = this.calcToAmount" @change="toAmount = this.calcToAmount"/>
            <CustomSelect 
            :currencies="currencies"
            :currency="fromCurrency"
            @currency-selected="fromCurrency = $event; calculateRate()" 
            />
         </div>

         <div class="arrow" @click="this.switchCurrencies" tabindex="0">
            <i class="fas fa-arrow-right"></i>
         </div>

         <div class="area2">
            <label for="converted-to" class="label">Converted to:</label>
            <input type="number" id="converted-to" inputmode="decimal" tabindex="0" name="converted-to" v-model="toAmount" @keyup="fromAmount = calcFromAmount" @change="fromAmount = calcFromAmount"/>
            <CustomSelect 
            :currencies="currencies"
            :currency="toCurrency"
            @currency-selected="toCurrency = $event; calculateRate()" />
         </div>

         <div class="show-rate">
            <p>
               1 {{ fromCurrency.code }} = <span class="rate">{{ rate }}</span> {{ toCurrency.code }}
            </p>
            <span>
               Rates are updated daily at 16.00 CET by the European Central Bank <br/>
               <a href="https://exchangeratesapi.io/">https://exchangeratesapi.io/</a>
            </span>
         </div>
      </div>
   </div>
</template>

<script>
import CustomSelect from './components/CustomSelect.vue'
export default {
   name: 'App',
   data () {
      return {
         currencies:[
            {
               name: "British Pound Sterling",
               search: "GBP UK BRITISH POUND ",
               code: "GBP",
               flag:  "./flags/gbp.png"
            },
            {
               name:"US Dollars",
               search: "USD USA AMERICAN DOLLAR ",
               code: "USD",
               flag: "./flags/usd.png"
            },
            {
               name: "Euro",
               search: "EURO EUROPE",
               code: "EUR",
               flag:  "./flags/eur.png"
            },
            {
               name:"Swiss Francs",
               search: "CHF SWISS FRANC ",
               code: "CHF",
               flag:  "./flags/chf.png"
            },
            {
               name:"Canadian Dollars",
               search: "CAD CANADIAN DOLLAR ",
               code: "CAD",
               flag:  "./flags/cad.png"
            },
            {
               name: "Hong Kong Dollars",
               search: "HKD HONG KONG DOLLAR ",
               code: "HKD",
               flag:  "./flags/hkd.png"
            },
            {
               name: "Japanese Yen",
               search: "JPY JAPANESE YEN ",
               code: "JPY",
               flag:  "./flags/jpy.png"
            },
            {
               name: "Australian Dollar",
               search: "AUD AUSTRALIAN DOLLAR ",
               code: "AUD",
               flag:  "./flags/aud.png"
            },
            {
               name: "Indian Rupee",
               search: "INR INDIAN RUPEE ",
               code: "INR",
               flag:  "./flags/inr.png"
            },
            {
               name: "Chinese Yuan",
               search: "CNY CHINA CHINESE YUAN ",
               code: "CNY",
               flag:  "./flags/cny.png"
            },
            {
               name: "Icelandic Krona",
               search: "ISK ICELANDIC KRONA ",
               code: "ISK",
               flag:  "./flags/isk.png"
            },
            {
               name: "Philippine Peso",
               search: "PHP PHILIPPINE PESO ",
               code: "PHP",
               flag:  "./flags/php.png"
            },
            {
               name: "Danish Krone",
               search: "DKK DENMARK DANISH KRONE ",
               code: "DKK",
               flag:  "./flags/dkk.png"
            },
            {
               name: "Hungarian Forint",
               search: "HUF HUNGARIAN FORINT ",
               code: "HUF",
               flag:  "./flags/huf.png"
            },
            {
               name: "Czech Koruna",
               search: "CZK CZECH KORUNA ",
               code: "CZK",
               flag:  "./flags/czk.png"
            },
            {
               name: "Romanian Leu",
               search: "RON ROMANIAN LEU ",
               code: "RON",
               flag:  "./flags/ron.png"
            },
            {
               name: "Swedish Krona",
               search: "SEK SWEDISH KRONA ",
               code: "SEK",
               flag:  "./flags/sek.png"
            },
            {
               name: "Indonesian Rupiah",
               search: "IDR INDONESIAN RUPIAH ",
               code: "IDR",
               flag:  "./flags/idr.png"
            },
            {
               name: "Brazilian Real",
               search: "BRL BRAZILIAN REAL ",
               code: "BRL",
               flag:  "./flags/brl.png"
            },
            {
               name: "Russian Ruble",
               search: "RUB RUSSIAN RUBLE ",
               code: "RUB",
               flag:  "./flags/rub.png"
            },
            {
               name: "Croatian Kuna",
               search: "HRK CROATIAN KUNA ",
               code: "HRK",
               flag:  "./flags/hrk.png"
            },
            {
               name: "Thai Baht",
               search: "THB THAI BAHT ",
               code: "THB",
               flag:  "./flags/thb.png"
            },
            {
               name: "Singaporean Dollars",
               search: "SGD SINGAPOREAN DOLLAR ",
               code: "SGD",
               flag:  "./flags/sgd.png"
            },
            {
               name: "Polish Zloty",
               search: "PLN POLISH ZLOTY ",
               code: "PLN",
               flag:  "./flags/pln.png"
            },
            {
               name: "Bulgarian Lev",
               search: "BGN BULGARIAN LEV ",
               code: "BGN",
               flag:  "./flags/bgn.png"
            },
            {
               name: "Turkish Lira",
               search: "TRY TURKISH LIRA ",
               code: "TRY",
               flag:  "./flags/try.png"
            },
            {
               name: "Norwegian Krone",
               search: "NOK NORWEGIAN KRONE ",
               code: "NOK",
               flag:  "./flags/nok.png"
            },
            {
               name: "New Zealand Dollars",
               search: "NZD NEW ZEALAND DOLLAR ",
               code: "NZD",
               flag:  "./flags/nzd.png"
            },
            {
               name: "South African Rand",
               search: "ZAR SOUTH AFRICAN RAND ",
               code: "ZAR",
               flag:  "./flags/zar.png"
            },
            {
               name: "Mexican Peso",
               search: "MXN MEXICAN PESO ",
               code: "MXN",
               flag:  "./flags/mxn.png"
            },
            {
               name: "Israeli New Shekel",
               search: "ILS ISRAELI NEW SHEKEL",
               code: "ILS",
               flag:  "./flags/ils.png"
            },
            {
               name: "South Korean Won",
               search: "KRW SOUTH KOREAN WON",
               code: "KRW",
               flag:  "./flags/krw.png"
            },
            {
               name: "Malaysian Ringgit",
               search: "MYR MALAYSIAN RINGGIT",
               code: "MYR",
               flag:  "./flags/myr.png"
            }
         ],
         rate: "",
         allRates: "",
         fromAmount: "100",
         toAmount: "",
         selectOpen: false,
         fromCurrency:
            {
               name: "British Pound Sterling",
               code: "GBP",
               flag:  "./flags/gbp.png"
            },
         toCurrency:
            {
               name: "US Dollars",
               code: "USD",
               flag: "./flags/usd.png"
            }
      }
   },

   computed: {
      calcToAmount () {
         return (this.fromAmount * this.rate).toFixed(2)
      },
      calcFromAmount () {
         return (this.toAmount / this.rate).toFixed(2)
      }
   },

   methods: {
      fetchRate () {
         fetch(`http://api.exchangeratesapi.io/v1/latest?access_key=48a79b0740607e90cf3fa108af087d2d&format=1`)
         .then( res => res.json() )
         .then( data => {
            this.allRates = data.rates
            this.rate = (this.allRates.USD / this.allRates.GBP)
            const to = this.rate * this.fromAmount
            this.toAmount = to.toFixed(2)
         })
         .catch(err => console.error(err))
      },

      calculateRate () {
         const fromCode = this.fromCurrency.code
         const toCode = this.toCurrency.code
         this.rate = this.allRates[toCode] / this.allRates[fromCode]
         this.toAmount = (this.fromAmount * this.rate).toFixed(2)
      },

      switchCurrencies () {
         const originalFromAmount = this.fromAmount
         const orginalFromCurrency = this.fromCurrency

         this.fromAmount = this.toAmount
         this.toAmount = originalFromAmount
         this.fromCurrency = this.toCurrency
         this.toCurrency = orginalFromCurrency
         this.calculateRate()
      }
   },
   
   mounted: function () {
      this.fetchRate()
   },

   components: {CustomSelect},
}
</script>

<style>
body {
   background-image: url("../public/bg.png");
   background-repeat: repeat-x;
   background-color: rgb(225, 231, 237);
}
.page {
   margin: auto;
   max-width: 1000px;
   font-family: 'Open Sans', sans-serif;
   color: #263a66;
   display: flex;
   flex-direction: column;
}
h1 {
   color: white;
   font-size: 36px;
   margin: 60px auto 10px 30px;
}
h2 {
   color: #c9c9c9;
   margin: 5px auto 25px 30px;
   font-weight: 400;
}
.converter {
   display: grid;
   grid-template-columns: 40px 1fr 60px 1fr 40px;
   grid-template-rows: 40px 1fr 1fr 40px;
   grid-template-areas:
      ". . . . ."
      ". area1 arrow area2 ."
      ". rates rates rates ."
      ". . . . .";
   border-radius: 4px;
   color: #263a66;
   background: white;
   box-shadow: 0 0 10px #666;
}
.arrow {
   grid-area: arrow;
   margin: auto;
   outline: none;
}
.arrow:hover, .arrow:focus{
   cursor: pointer;
   color: #00c7cb;
}
.area1 { grid-area: area1; }
.area2 { grid-area: area2; }
.area1:focus-within > label, .area2:focus-within > label  {
   color: #00c7cb;
}

.area1, .area2 {
   display: grid;
   grid-template-columns: 1fr 120px;
   grid-template-rows: 25% 75%;
   grid-template-areas: 
      "label ."
      "amount select";
}
.area1.label, .area2.label { grid-area: label; }
.area1 input, .area2 input { grid-area: amount}
.area1 div.custom-select, .area2 div.custom-select {grid-area: select;}

.show-rate {
   font-size: 22px;
   grid-area: rates;
}
.show-rate p{
   margin: 10px auto;
}
.show-rate span {
   font-size: 12px;;
}
span.rate {
   color:#00c7cb;
   font-size: 22px;
}
label {
   color: #263a66;
   font-size: 14px;
   margin: 5px 10px;
}
input {
   padding: 0 10px;
   height: 60px;
   margin: 5px;
   font-size: 18px;
   border: 1px #ccc solid;
   border-radius: 3px;
   color: #263a66;
   outline-color: #00e7eb;
}
input[type=number]::-webkit-inner-spin-button, 
input[type=number]::-webkit-outer-spin-button { 
  -webkit-appearance: none; 
  margin: 0; 
}
input:hover {
   border: 1px black solid;
}
a{color: #47a97c;}


@media only screen and (max-width: 830px) {
   .converter {
      grid-template-columns: 40px 1fr 40px;
      grid-template-rows: 40px 1fr 30px 1fr 1.2fr 40px;
      grid-template-areas:
         ". . ."
         ". area1 ."
         ". arrow ."
         ". area2 ."
         ". rates ."
         ". . .";
   }
   h1 {
      font-size: 30px;
      margin: 60px 30px 0 30px;
   }
   h2 {
      margin: 5px 30px 25px 30px;
      font-size: 20px;
   }
}
@media only screen and (max-width: 550px) {
   .converter { grid-template-columns: 20px 1fr 20px;}
   h1 {
      font-size: 24px;
      margin: 10px 10px 0 10px;
   }
   h2 {
      margin: 5px 10px 25px 10px;
      font-size: 16px;
   }
}
@media only screen and (max-width: 400px) {
   input{width: 170px;} 
}
@media only screen and (max-width: 380px) {
   input{width: 150px;}
}
@media only screen and (max-width: 360px) {
   input{width: 130px;}
}
@media only screen and (max-width: 340px) {
   input{width: 110px;}
}
</style>
