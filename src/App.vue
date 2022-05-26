<template>
  <div>
    <div v-if="recievedData">
      <ul v-for="entry in recievedData">
        <li>{{entry.commit.author.name}} <br> {{entry.commit.author.date}} <br>
         {{entry.commit.message}}</li>
        <br>
    
      </ul>
    </div>
    <div v-else>
      <p>Loading...</p>
    </div>
  </div>
</template>

<script type="module">
import { processExpression } from "@vue/compiler-core";
import { Octokit, App } from "https://cdn.skypack.dev/octokit"

//yay it works

export default {
  async mounted(){
    try{ 

      const octokit = new Octokit({
        //make backend request
        auth: import.meta.env.MYPAT
      })
      
      var response = await octokit.request('GET /repos/{owner}/{repo}/commits', {
        owner: 'srod5125',
        repo: 'roads_sub'
      })

      this.recievedData = response.data
      console.log("response all")
      console.log(response)
      console.log("response data")
      console.log(response.data)
      console.log("data length")
      console.log(this.recievedData.length)

      
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
