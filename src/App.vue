<template>
  <div>
    <button @click="GetCommits">Load Data</button>

    <div v-if=" state=='USERLANDED' ">
      <p>{{message}}</p>
    </div>
    <div v-else-if=" state=='GOTDATA' && recievedData ">
      <div>
        <p>{{message}}</p> 
      </div>
      <br>
      <ul v-for="entry in recievedData">
        <li>{{entry.commit.author.name}} <br> {{entry.commit.author.date}} <br>
         {{entry.commit.message}}</li>
        <br>
    
      </ul>
    </div>

  </div>
</template>

<script type="module">
import { processExpression } from "@vue/compiler-core";
import { Octokit, App } from "https://cdn.skypack.dev/octokit"

//yay it works

export default {
  async mounted(){
    
  },
  methods:{ 
    async GetCommits () { 
      try{ 

        this.message = "Loading..."

        const octokit = new Octokit({
          //make backend request
          auth: import.meta.env.MYPAT
        })
        
        var response = await octokit.request('GET /repos/{owner}/{repo}/commits', {
          owner: 'srod5125',
          repo: 'roads_sub'
        })

        this.recievedData = response.data

        //console.log("response all")
        //console.log(response)
        //console.log("response data")
        //console.log(response.data)
        //console.log("data length")
        //console.log(this.recievedData.length)

        this.state = 'GOTDATA'
        this.message = "Got commits"

      }
      catch(error){ 
        console.log(error)
        this.message = "An error occurred"
      }
    }
  },
  data(){
    return{ 
      recievedData:null,
      state:'USERLANDED',
      message:"Nothing yet"
    }
  }
}
</script>

<style>
</style>
