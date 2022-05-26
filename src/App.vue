<template>
  <div class="container">

    <h1 class="titleHead">Crossroads Project Submission</h1>

    <div class="leftUp">
      <button @click="GetCommits"
      v-bind:disabled="state=='GOTDATA' && recievedData"
      class="button-1">Load Data</button>
    </div>
    
    <!--body-->
    <div class="core">
      <div v-if=" state=='USERLANDED' ">
        <p>{{message}}</p>
      </div>
      <div v-else-if=" state=='GOTDATA' && recievedData ">
        <div>
          <p>{{message}}</p> 
        </div>
        <br>
        <!-- table begining-->
        <table class="customTable">
          <thead>
            <tr>
              <th>Message</th>
              <th>Date</th>
            </tr>
          </thead>
          <tbody v-for="entry in recievedData">
            <tr>
              <td>{{entry.commit.message}}</td>
              <td>{{entry.commit.author.date}}</td>
            </tr>
          </tbody>
        </table>
        <!-- table end-->
      </div>
    </div>
    <!--body end-->
    <div class="footnote">
      <p class="b">
        Author:Stephen Rodriguez
      </p> 
      <p class="b">
        Email: srod5125@gmail.com
      </p>
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

        this.state = 'GOTDATA'
        this.message = "Got commits!!"

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
  },
  created: function () {
    document.body.style.backgroundColor = "#ff9933";
  },
  destroyed: function () {
    document.body.style.backgroundColor = null;
  }
}
</script>

<style>
  .container {
    display: grid;
    grid-template-columns: repeat(5, 1fr);
    grid-template-rows: 90px repeat(4, 1fr);
    grid-column-gap: 12px;
    grid-row-gap: 12px;

    /*background-color: #ff9933;*/
  }

  .titleHead { 
    grid-area: 1 / 2 / 2 / 5;
    text-align: center;
    }
  .leftUp { grid-area: 2 / 1 / 3 / 2; }
  .core { grid-area: 2 / 2 / 6 / 5; }
  .leftBottom { grid-area: 3 / 1 / 4 / 2; }
  .footnote { grid-area: 5 / 2 / 6 / 5; }
  p { 
    text-align: center;
    font-size: 20px;
    font-family: 'Courier New', Courier, monospace;
     font-weight: bold;
  }
  p.b {
  font-style: italic;
  font-weight: normal;
}

/* CSS from online */
.button-1 {
  background-color: #EA4C89;
  border-radius: 8px;
  border-style: none;
  box-sizing: border-box;
  color: #FFFFFF;
  cursor: pointer;
  
  font-family: "Haas Grot Text R Web", "Helvetica Neue", Helvetica, Arial, sans-serif;
  font-size: 14px;
  font-weight: 500;
  height: 40px;
  line-height: 20px;
  list-style: none;
  margin: 0;
  outline: none;
  padding: 10px 16px;
  float:right;
  text-align: center;
  text-decoration: none;
  transition: color 100ms;
  vertical-align: baseline;
  user-select: none;
  -webkit-user-select: none;
  touch-action: manipulation;
}

  .button-1:hover,
  .button-1:focus {
    background-color: #F082AC;
  }
  .button-1:disabled{ 
    background-color: grey;
    cursor:default;
  }

  /* online table creation */
  table.customTable {
    width: 100%;
    background-color: #FFFFFF;
    border-collapse: collapse;
    border-width: 2px;
    border-color: #7EA8F8;
    border-style: solid;
    color: #000000;
  }

  table.customTable td, table.customTable th {
    border-width: 2px;
    border-color: #7EA8F8;
    border-style: solid;
    padding: 5px;
  }

  table.customTable thead {
    background-color: #7EA8F8;
  }
</style>
