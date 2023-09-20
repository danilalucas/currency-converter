<template>
  <section class="converted">
    <div class="container">
      <h1>Conversor de Moedas</h1>
      <p>Precisa fazer um pagamento comercial internacional? Dê uma olhada em nossas taxas de câmbio ao vivo.</p>
      <div class="card-container">
        <div class="card-description">
          <h2>Faça pagamentos comerciais internacionais rápidos e acessíveis</h2>
          <p>Envie pagamentos comerciais internacionais seguros em qualquer moedas, tudo com taxas competitivas e sem taxas ocultas.</p>
        </div>
        <div class="card-form">
          <input v-model="value" type="number" id="value">
          <select v-model="currencyOrigin">
            <option v-for="myOption in myOptions" :value="myOption.value">{{myOption.title}}</option>
          </select>
          <button @click="invert" id="invert" ><img src="./assets/invert.png" alt=""></button>
          <input v-model="result" type="number" id="converted_to" disabled>
          <select v-model="currencyDestination">
            <option v-for="myOption in myOptions" :value="myOption.value">{{myOption.title}}</option>
          </select>
        </div>
        <div class="card-footer">
          <p>{{ tax }}</p>
          <button @click="converter" id="converted" >Converter</button>
        </div>
      </div>
    </div>
  </section>
</template>

<script>

  export default {
    data() {
      return {
        currencyOrigin: 'USD',
        currencyDestination: 'BRL',
        value: 1,
        result: '',
        myOptions: [
          {'value':'USD', 'title':'USD (Dólar dos EUA)'},
          {'value':'BRL', 'title':'BRL (Real Brazil)'},
          {'value':'CAD', 'title':'CAD (Dollar Canada)'},
          {'value':'JPY', 'title':'JPY (Iene Japonês)'},
          {'value':'EUR', 'title':'EUR (Euro)'},
          {'value':'AUD', 'title':'AUD (Dollar Australia)'},
          {'value':'CUP', 'title':'CUP (Peso Cuba)'},
          {'value':'CLP', 'title':'CLP (Peso Chile)'},
          {'value':'CNY', 'title':'CNY (Renminbi	China)'},
          {'value':'HKD', 'title':'HKD (Dollar Hong Kong)'},
          {'value':'RUB', 'title':'RUB (Ruble Russia)'},
          {'value':'INR', 'title':'INR (Rupee India)'},
        ],
      };
    },
    mounted() {
      this.converter();
    },
    methods: {
      invert() {
        [this.currencyOrigin, this.currencyDestination] = [this.currencyDestination, this.currencyOrigin];
        [this.value, this.result] = [this.result, this.value];
      },
      converter() {
        const baseUrl = import.meta.env.VITE_API_BASE_URL;
        const apiKey = import.meta.env.VITE_API_KEY;
        const url = `${baseUrl}/${apiKey}/pair/${this.currencyOrigin}/${this.currencyDestination}`;

        fetch(url)
        .then((response) => response.json())
        .then((data) => {
          if (data.error) {
            this.result = 'Erro ao obter as taxas de câmbio.';
          } else {
            const taxaCambio = data.conversion_rate;
            this.result = (this.value * taxaCambio).toFixed(2);
            this.tax = `1.00 ${this.currencyOrigin} = ${taxaCambio} ${this.currencyDestination}`
          }
        })
        .catch((error) => {
          this.result = error;
        });
      }
    }
  };
</script>
