<template>
  <div class="container mx-auto mt-3">
      <p v-if="$fetchState.pending">Fetching Exchanges...</p>
      <p v-else-if="$fetchState.error">An error occured</p>
      <div v-else>
          <h6 class="text-center">Top Ten Exchanges</h6>
          <ul>
            <li ></li>
          </ul>
          <!-- This example requires Tailwind CSS v2.0+ -->
          <div class="flex flex-col">
            <div class="-my-2 overflow-x-auto sm:-mx-6 lg:-mx-8">
              <div class="py-2 align-middle inline-block min-w-full sm:px-6 lg:px-8">
                <div class="shadow overflow-hidden border-b border-gray-200 sm:rounded-lg">
                  <table class="min-w-full divide-y divide-gray-200">
                    <thead class="bg-gray-50">
                      <tr>
                        <th scope="col" class="px-6 py-3 text-left text-xs font-medium text-gray-500 uppercase tracking-wider">
                          #
                        </th>
                        <th scope="col" class="px-6 py-3 text-left text-xs font-medium text-gray-500 uppercase tracking-wider">
                          Name
                        </th>
                        <th scope="col" class="px-6 py-3 text-left text-xs font-medium text-gray-500 uppercase tracking-wider">
                          Established
                        </th>
                        <th scope="col" class="px-6 py-3 text-left text-xs font-medium text-gray-500 uppercase tracking-wider">
                          Trust Score
                        </th>
                        <th scope="col" class="px-6 py-3 text-left text-xs font-medium text-gray-500 uppercase tracking-wider">
                          Trust Rank
                        </th>
                        <th scope="col" class="px-6 py-3 text-left text-xs font-medium text-gray-500 uppercase tracking-wider">
                          24 Hour Volume (Normalized)
                        </th>
                        <th scope="col" class="px-6 py-3 text-left text-xs font-medium text-gray-500 uppercase tracking-wider">
                          24 Hour Volume
                        </th>
                      </tr>
                    </thead>
                    <tbody class="bg-white divide-y divide-gray-200">
                      <tr v-for="exchange of exchanges" :key="exchange.id">
                        <td class="px-6 py-4 whitespace-nowrap">
                          <div class="text-sm text-gray-900">1</div>
                        </td>
                        <td class="px-6 py-4 whitespace-nowrap">
                          <div class="flex items-center">
                            <div class="flex-shrink-0 h-10 w-10">
                              <img class="h-10 w-10 rounded-full" :src="exchange.image" alt="">
                            </div>
                            <div class="ml-4">
                              <div class="text-sm font-medium text-gray-900">
                                <a :href="exchange.url">{{ exchange.name }}</a>
                              </div>
                            </div>
                          </div>
                        </td>
                        <td class="px-6 py-4 whitespace-nowrap">
                          <div class="text-sm text-gray-900">{{ exchange.year_established }}</div>
                        </td>
                        <td class="px-6 py-4 whitespace-nowrap">
                          <span class="px-2 inline-flex text-xs leading-5 font-semibold rounded-full bg-green-100 text-green-800">
                            {{ exchange.trust_score }}
                          </span>
                        </td>
                        <td class="px-6 py-4 whitespace-nowrap text-sm text-gray-500">
                          {{ exchange.trust_score_rank }}
                        </td>
                        <td class="px-6 py-4 whitespace-nowrap text-sm text-gray-500">
                          {{ exchange.trade_volume_24h_btc_normalized.toFixed(2) }}
                        </td>
                        <td class="px-6 py-4 whitespace-nowrap text-sm text-gray-500">
                          {{ exchange.trade_volume_24h_btc.toFixed(2) }}
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
        exchanges:[]
      }
    },
    async fetch()
    {
      this.exchanges = await fetch(
        'https://api.coingecko.com/api/v3/exchanges?per_page=10&page=1'
      ).then(res => res.json())
    }
  }
</script>
