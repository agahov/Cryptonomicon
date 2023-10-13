<template>
  <div class="container flex flex-col items-center p-4 mx-auto bg-gray-100">
    <div v-if="isLoading"
      class="fixed inset-0 z-50 flex items-center justify-center bg-purple-800 w-100 h-100 opacity-80"
    >
      <svg
        class="w-12 h-12 mr-3 -ml-1 text-white animate-spin"
        xmlns="http://www.w3.org/2000/svg"
        fill="none"
        viewBox="0 0 24 24"
      >
        <circle
          class="opacity-25"
          cx="12"
          cy="12"
          r="10"
          stroke="currentColor"
          stroke-width="4"
        ></circle>
        <path
          class="opacity-75"
          fill="currentColor"
          d="M4 12a8 8 0 018-8V0C5.373 0 0 5.373 0 12h4zm2 5.291A7.962 7.962 0 014 12H0c0 3.042 1.135 5.824 3 7.938l3-2.647z"
        ></path>
      </svg>
    </div>
    <div class="container">
      <button @click.stop="loadTikers"> show api</button>
      
      <section>
        <div class="flex">
          <div class="max-w-xs">
            <label for="wallet" class="block text-sm font-medium text-gray-700">Тикер</label>
            <div class="relative mt-1 rounded-md shadow-md">
              <input
                v-model="ticker"
                @keydown.enter="add"
                type="text"
                name="wallet"
                id="wallet"
                class="block w-full pr-10 text-gray-900 border-gray-300 rounded-md focus:border-gray-500 focus:outline-none focus:ring-gray-500 sm:text-sm"
                placeholder="Например DOGE"
              />
            </div>
            <div class="flex flex-wrap p-1 bg-white rounded-md shadow-md">
              <span v-for="coin in coins"   @click.stop="ticker = coin"
                class="inline-flex items-center px-2 m-1 text-xs font-medium text-gray-800 bg-gray-300 rounded-md cursor-pointer"
              >
                {{coin}}
              </span>
            </div>
            <div class="text-sm text-red-600">Такой тикер уже добавлен</div>
          </div>
        </div>
        <button
          @click="add"
          type="button"
          class="inline-flex items-center px-4 py-2 my-4 text-sm font-medium leading-4 text-white transition-colors duration-300 bg-gray-600 border border-transparent rounded-full shadow-sm hover:bg-gray-700 focus:outline-none focus:ring-2 focus:ring-gray-500 focus:ring-offset-2"
        >
          <!-- Heroicon name: solid/mail -->
          <svg
            class="-ml-0.5 mr-2 h-6 w-6"
            xmlns="http://www.w3.org/2000/svg"
            width="30"
            height="30"
            viewBox="0 0 24 24"
            fill="#ffffff"
          >
            <path
              d="M13 7h-2v4H7v2h4v4h2v-4h4v-2h-4V7zm-1-5C6.48 2 2 6.48 2 12s4.48 10 10 10 10-4.48 10-10S17.52 2 12 2zm0 18c-4.41 0-8-3.59-8-8s3.59-8 8-8 8 3.59 8 8-3.59 8-8 8z"
            ></path>
          </svg>
          Добавить
        </button>
      </section>

       <template v-if="tickers.length">
      

      <hr class="w-full my-4 border-t border-gray-600" />
<div>
          <button
            class="inline-flex items-center px-4 py-2 mx-2 my-4 text-sm font-medium leading-4 text-white transition-colors duration-300 bg-gray-600 border border-transparent rounded-full shadow-sm hover:bg-gray-700 focus:outline-none focus:ring-2 focus:ring-offset-2 focus:ring-gray-500"
            v-if="page > 1"
            @click="page = page - 1"
          >
            Назад
          </button>
          <button
            class="inline-flex items-center px-4 py-2 mx-2 my-4 text-sm font-medium leading-4 text-white transition-colors duration-300 bg-gray-600 border border-transparent rounded-full shadow-sm hover:bg-gray-700 focus:outline-none focus:ring-2 focus:ring-offset-2 focus:ring-gray-500"
            @click="page = page + 1"
            v-if="hasNextPage"
          >
            Вперед
          </button>
          <!-- <div>Фильтр: <input v-model="filter" /></div> -->
        </div>

          <hr class="w-full my-4 border-t border-gray-600" />




      <dl class="grid grid-cols-1 gap-5 mt-5 sm:grid-cols-3">
        <div
          v-for = "t in filteredTickers"
          :key="t.name"
          @click="select(t)"
          :class="{'border-4':sel===t}"
          
          class="overflow-hidden bg-white border-purple-800 border-solid rounded-lg shadow cursor-pointer"
        >
          <div class="px-4 py-5 text-center sm:p-6">
            <dt class="text-sm font-medium text-gray-500 truncate">{{t.name}}- USD</dt>
            <dd class="mt-1 text-3xl font-semibold text-gray-900">{{ t.price }}</dd>
          </div>
          <div class="w-full border-t border-gray-200"></div>
          <buttons
            @click.stop="handleDelete(t)"
            class="flex items-center justify-center w-full px-4 py-4 font-medium text-gray-500 transition-all bg-gray-100 text-md hover:bg-gray-200 hover:text-gray-600 hover:opacity-20 focus:outline-none sm:px-6"
          >
            <svg
              class="w-5 h-5"
              xmlns="http://www.w3.org/2000/svg"
              viewBox="0 0 20 20"
              fill="#718096"
              aria-hidden="true"
            >
              <path
                fill-rule="evenodd"
                d="M9 2a1 1 0 00-.894.553L7.382 4H4a1 1 0 000 2v10a2 2 0 002 2h8a2 2 0 002-2V6a1 1 0 100-2h-3.382l-.724-1.447A1 1 0 0011 2H9zM7 8a1 1 0 012 0v6a1 1 0 11-2 0V8zm5-1a1 1 0 00-1 1v6a1 1 0 102 0V8a1 1 0 00-1-1z"
                clip-rule="evenodd"
              ></path>
            </svg>
            >Удалить
          </buttons>
        </div>
      </dl>
      <hr class="w-full my-4 border-t border-gray-600" />
</template>

      <section v-if="sel" class="relative" >
        <h3 class="my-8 text-lg font-medium leading-6 text-gray-900">{{sel.name}} - USD</h3>
        <div class="flex items-end h-64 border-b border-l border-gray-600">
        <div
            v-for="(bar, idx) in normalizeGraph()"
            :key="idx"
            :style="{ height: `${bar}%` }"
            class="w-10 bg-purple-800 border"
          ></div>

        </div>
        <button @click="sel = null" type="button" class="absolute top-0 right-0">
          <svg
            xmlns="http://www.w3.org/2000/svg"
            xmlns:xlink="http://www.w3.org/1999/xlink"
            xmlns:svgjs="http://svgjs.com/svgjs"
            version="1.1"
            width="30"
            height="30"
            x="0"
            y="0"
            viewBox="0 0 511.76 511.76"
            style="enable-background: new 0 0 512 512"
            xml:space="preserve"
          >
            <g>
              <path
                d="M436.896,74.869c-99.84-99.819-262.208-99.819-362.048,0c-99.797,99.819-99.797,262.229,0,362.048    c49.92,49.899,115.477,74.837,181.035,74.837s131.093-24.939,181.013-74.837C536.715,337.099,536.715,174.688,436.896,74.869z     M361.461,331.317c8.341,8.341,8.341,21.824,0,30.165c-4.16,4.16-9.621,6.251-15.083,6.251c-5.461,0-10.923-2.091-15.083-6.251    l-75.413-75.435l-75.392,75.413c-4.181,4.16-9.643,6.251-15.083,6.251c-5.461,0-10.923-2.091-15.083-6.251    c-8.341-8.341-8.341-21.845,0-30.165l75.392-75.413l-75.413-75.413c-8.341-8.341-8.341-21.845,0-30.165    c8.32-8.341,21.824-8.341,30.165,0l75.413,75.413l75.413-75.413c8.341-8.341,21.824-8.341,30.165,0    c8.341,8.32,8.341,21.824,0,30.165l-75.413,75.413L361.461,331.317z"
                fill="#718096"
                data-original="#000000"
              ></path>
            </g>
          </svg>
        </button>
      </section>
    </div>
  </div>
</template>

<script>



export default{
  name: "App",


  created(){
          console.log("... created  ")

    const tickersData = localStorage.getItem("cryptonomicon-list");
    if (tickersData) {
      this.tickers = JSON.parse(tickersData);
      // this.tickers.forEach(ticker => {
      //   this.subscribeToUpdates(ticker.name);
      // });
    }


      
      




  },
   
    async beforeMount()
    {    
      console.log("before mounte  ")

      const f =  await fetch("https://min-api.cryptocompare.com/data/all/coinlist?summary=true");
      const data = await f.json();
      
      console.log(data)

        let start = 0
        for (let key in data.Data){
          start++
          if (start>100){
            this.coins.push(key)
            if (this.coins.length>5){
              this.isLoading = false
              return
            }
          }
      
          
        } 

      setTimeout(this.isLoading = false,100);

      this.coins.push("BTC", "USDT", "ETH")

      
  }
  ,

  computed: {


  }
  ,

  data() {
    return {
      isLoading:true,
      page:1,
      hasNextPage: true,
      ticker: "default",
      tickers: [
      ],
      sel:null,
      graph:[],
      coins:[]
    };
  },

  computed:{

    filteredTickers:function ()  {

    if (!this.tickers)
    {return []}

      const start = (this.page - 1) * 6;
      const end = this.page * 6;

      const filteredTickers = this.tickers.filter(ticker =>
        ticker.name.includes(this.filter)
      );

      this.hasNextPage = filteredTickers.length > end;

      return filteredTickers.slice(start, end);
    },
 

   

  },  

  methods: {

// filteredTickers:function ()  {

//     if (!this.tickers)
//     {return []}

//       const start = (this.page - 1) * 6;
//       const end = this.page * 6;

//       const filteredTickers = this.tickers.filter(ticker =>
//         ticker.name.includes(this.filter)
//       );

//       this.hasNextPage = filteredTickers.length > end;

//       return filteredTickers.slice(start, end);
//     },
 

 subscribeToUpdates(tickerName) {
      setInterval(async () => {
        const f = await fetch(
          `https://min-api.cryptocompare.com/data/price?fsym=${tickerName}&tsyms=USD&api_key=ce3fd966e7a1d10d65f907b20bf000552158fd3ed1bd614110baa0ac6cb57a7e`
        );
        const data = await f.json();

        // currentTicker.price =  data.USD > 1 ? data.USD.toFixed(2) : data.USD.toPrecision(2);
        console.log(" subscribeToUpdates") ;

        console.log(data);
          
       if (data.USD){

            this.tickers.find(t => t.name === tickerName).price =
              data.USD > 1 ? data.USD.toFixed(2) : data.USD.toPrecision(2);

            if (this.sel?.name === tickerName) {
              this.graph.push(data.USD);
            }
          }    

          }, 5000);

        
      this.ticker = "";
    },

    add() {
      const currentTicker = {
        name: this.ticker,
        price: "-"
      };

      this.tickers.push(currentTicker);
      this.filter = "";

      localStorage.setItem("cryptonomicon-list", JSON.stringify(this.tickers));
      this.subscribeToUpdates(currentTicker.name);
    },

    handleDelete(tickerToRemove) {
      this.tickers = this.tickers.filter(t => t !== tickerToRemove);
    },
     select(ticker) {
      this.sel = ticker;
      this.graph = [];
    },


    normalizeGraph() {
      const maxValue = Math.max(...this.graph);
      const minValue = Math.min(...this.graph);
      return this.graph.map(
        price => 5 + ((price - minValue) * 95) / (maxValue - minValue)
      );
    },


    loadTikers(){
      console.log("test")
      console.log(import.meta.env.VITE_CRYPTOCOMPARE_APIKEY)
    },
  }
};

</script>




