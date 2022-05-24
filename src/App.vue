<template>
  <pre>{{data}}</pre>
</template>

<script type="module">
import { Octokit, App } from "https://cdn.skypack.dev/octokit"

export default {
  async mounted(){
    try{ 
      const octokit = new Octokit({
      auth: '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'
      })

      let response = await octokit.request('GET /repos/{owner}/{repo}/commits', {
        owner: 'srod5125',
        repo: 'roads_sub'
      })
      var stringedRes = JSON.stringify(response)

      console.log(stringedRes)
      this.data = stringedRes

    }
    catch(error){ 
      console.log(error)
    }
  },
  data(){
    return{ 
      data:""
    }
  }
}
</script>

<style>
</style>
