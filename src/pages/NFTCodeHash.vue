<template>
  <div>
    <h3> NFT CodeHash Summary:
    </h3>

    <table class="table">
      <thead>
        <tr>
          <th class="text-right">#</th>
          <th class="text-center">codehash (see all genesis)</th>
          <th class="text-right">NFT total</th>
          <th class="text-right">in times</th>
          <th class="text-right">out times</th>
        </tr>
      </thead>
      <tbody>
        <tr v-for="(info, index) in codeHashInfo">
          <td class="text-right"><samp>{{ index }}</samp></td>
          <td class="text-center">
            <router-link :to="{path:`/nft/codehash/${info.codehash}`}">
              <samp>{{ info.codehash }}</samp>
            </router-link>
          </td>
          <td class="text-right">
            <samp>{{ info.count }}</samp>
          </td>
          <td class="text-right">
            <samp>+{{ info.inTimes }}</samp>
          </td>
          <td class="text-right">
            <code>-{{ info.outTimes }}</code>
          </td>
        </tr>
      </tbody>
    </table>
  </div>
</template>

<script>
  import axios from 'axios'

  export default {
    data: function () {
      return {
        currGenesis: "",
        codeHashInfo: []
      }
    },

    watch: {
      $route(to, from) {
        console.log("watch", this.$route)

        if (this.codeHashInfo === undefined) {
          this.codeHashInfo = []
        }

        if (to.path != "/genesis" ) {
          this.viewTxOutpointByGenesis(to.params.genesis)
        }
      }
    },

    created: function () {
      console.log("create", this.$route)

      if (this.codeHashInfo === undefined) {
        this.codeHashInfo = []
      }
      this.viewTxOutpointByGenesis()
    },

    methods: {
      viewTxOutpointByGenesis: function () {
        this.codeHashInfo = []
        this.$root.message = "..."
        axios
          .get(this.$root.apiPoint + "nft/codehash/all")
          .then(
            response => {
              if (response.data.code == 0) {
                this.codeHashInfo = response.data.data
                this.$root.message = response.data.msg
              } else {
                this.$root.message = "empty history"
              }
            }
          )
      },
    }

  }
</script>
