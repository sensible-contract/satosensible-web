<template>
  <div>
    <h3> FT CodeHash Info:
      <small>
        <samp>{{ currCodeHash }}</samp>
      </small>
    </h3>

    <table class="table">
      <thead>
        <tr>
          <th class="text-right">#</th>
          <th class="text-center">genesis (see all owners)</th>
          <th class="text-right">FT total</th>
          <th class="text-right">in volumes</th>
          <th class="text-right">out volumes</th>
        </tr>
      </thead>
      <tbody>
        <tr v-for="(info, index) in currCodeHashInfos">
          <td class="text-right"><samp>{{ index }}</samp></td>
          <td class="text-left">
            <router-link :to="{path:`/ft/owners/${info.codehash}/${info.genesis}`}">
              <samp>{{ info.genesis }}</samp>
            </router-link>
          </td>
          <td class="text-right">
            <samp>{{ info.count }}</samp>
          </td>
          <td class="text-right">
            <samp>+{{ info.inVolume }}</samp>
          </td>
          <td class="text-right">
            <code>-{{ info.outVolume }}</code>
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
        currCodeHash: "",
        currCodeHashInfos: []
      }
    },

    watch: {
      $route(to, from) {
        console.log("watch", this.$route, this.currCodeHash)

        if (this.currCodeHashInfos === undefined) {
          this.currCodeHashInfos = []
        }

        if (to.path != "/ft/codehash" ) {
          this.viewInfoByCodeHash(to.params.codehash)
        }
      }
    },

    created: function () {
      console.log("create", this.$route, this.currCodeHash)

      if (this.currCodeHashInfos === undefined) {
        this.currCodeHashInfos = []
      }
      if (this.$route.path != "/ft/codehash") {
        this.viewInfoByCodeHash(this.$route.params.codehash)
      }
    },

    methods: {
      viewInfoByCodeHash: function (codehash) {
        console.log(codehash)
        this.currCodeHash = codehash
        this.currCodeHashInfos = []
        this.$root.message = "..."
        axios
          .get(this.$root.apiPoint + "ft/codehash-info/"+ codehash)
          .then(
            response => {
              if (response.data.code == 0) {
                this.currCodeHashInfos = response.data.data
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
