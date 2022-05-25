<template>
  <div>
    <div v-if="recievedData">
      <li v-for="(v, index) in recievedData">
        {{recievedData[index].commit}} 
        {{recievedData[index].author}}
        {{recievedData[index].committer}}
      </li>
    </div>
    <div v-else>
      <p>Loading...</p>
    </div>
  </div>
</template>

<script type="module">
import { Octokit, App } from "https://cdn.skypack.dev/octokit"

export default {
  async mounted(){
    try{ 
      const octokit = new Octokit({
        auth: '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'
      })

      var response = await octokit.request('GET /repos/{owner}/{repo}/commits', {
        owner: 'srod5125',
        repo: 'roads_sub'
      })
      //var stringedRes = JSON.stringify(response.data[0])

      this.recievedData = response.data
      console.log(Object.keys(response.data[0]))
      
    }
    catch(error){ 
      console.log(error)
    }
  },
  data(){
    return{ 
      recievedData:null
    }
  }
}
</script>

<style>
</style>
