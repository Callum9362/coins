<template>
   <div class="container mx-auto mt-3 px-4">
      <p v-if="$fetchState.pending">Fetching Coins...</p>
      <p v-else-if="$fetchState.error">An error occured</p>
      <div v-else>
          <div class="flex flex-col">
            <div class="-my-2 overflow-x-auto sm:-mx-6 lg:-mx-8">
              <div class="py-2 align-middle inline-block min-w-full sm:px-6 lg:px-8">
                <div class="shadow overflow-hidden border-b border-gray-200 sm:rounded-lg">
                  <table class="min-w-full divide-y divide-gray-200">
                    <thead class="bg-gray-50">
                      <tr>
                        <th scope="col" class="px-6 py-3 text-left text-xs font-medium text-gray-500 uppercase tracking-wider">
                          Name
                        </th>
                        <th scope="col" class="px-6 py-3 text-left text-xs font-medium text-gray-500 uppercase tracking-wider">
                          Symbol
                        </th>
                        <th scope="col" class="px-6 py-3 text-left text-xs font-medium text-gray-500 uppercase tracking-wider">
                          Price
                        </th>
                        <th scope="col" class="px-6 py-3 text-left text-xs font-medium text-gray-500 uppercase tracking-wider">
                          24h
                        </th>
                        <th scope="col" class="px-6 py-3 text-left text-xs font-medium text-gray-500 uppercase tracking-wider">
                          Market Cap
                        </th>
                      </tr>
                    </thead>
                    <tbody class="bg-white divide-y divide-gray-200">
                      <tr v-for="coin of coins" :key="coin.id">
                        <td class="px-6 py-4 whitespace-nowrap">
                          <div class="flex items-center">
                            <div class="flex-shrink-0 h-10 w-10">
                              <img class="h-10 w-10 rounded-full" :src="coin.image" alt="coin image">
                            </div>
                            <div class="ml-4">
                              <div class="text-sm font-medium text-gray-900">
                                {{ coin.name }}
                              </div>
                            </div>
                          </div>
                        </td>
                        <td class="px-6 py-4 whitespace-nowrap">
                          <div class="flex">
                              <div class="text-sm font-medium uppercase text-gray-900">
                                {{ coin.symbol }}
                              </div>
                          </div>
                        </td>
                        <td class="px-6 py-4 whitespace-nowrap">
                          <div class="flex">
                              <div class="text-sm" v-bind:class="highlight(coin.price_change_percentage_24h)">
                                ${{ coin.current_price.toLocaleString() }}
                              </div>
                          </div>
                        </td>
                        <td class="px-6 py-4 whitespace-nowrap">
                          <div class="flex">
                              <div class="text-sm" v-bind:class="highlight(coin.market_cap_change_percentage_24h)">
                                {{ coin.market_cap_change_percentage_24h.toFixed(1) }}
                              </div>
                          </div>
                        </td>
                        <td class="px-6 py-4 whitespace-nowrap">
                          <div class="flex">
                              <div class="text-sm">
                                ${{ coin.market_cap_change_24h.toLocaleString() }}
                              </div>
                          </div>
                        </td>
                      </tr>
                    </tbody>
                  </table>
                </div>
              </div>
            </div>
          </div>
      </div>
  </div>

</template>

<script>
  export default{
    data(){
      return {
        coins:[]
      }
    },
    methods:{
      highlight(priceChange){
        return {
          'text-red-500': priceChange < 0,
          'text-green-500': priceChange > 0
        }
      }
    },
    async fetch()
    {
      this.coins = await fetch(
        'https://api.coingecko.com/api/v3/coins/markets?vs_currency=usd&order=market_cap_desc&per_page=100&page=1'
      ).then(res => res.json())
    }
  }
</script>
