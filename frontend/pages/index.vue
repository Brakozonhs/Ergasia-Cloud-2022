<template>
  <section class="elk">
      <div class="search">
        <h2> Search for Logs...</h2>
          <b-field label="Key"
          type="is-success"
         >
          <b-input v-model="logKey" maxlength="30"  placeholder="Enter log property, with dots for nested properties etc. container.name"></b-input>
        </b-field>
        <b-field label="Value"
        type="is-success"
        >
        <b-input v-model="logValue" maxlength="30" placeholder="Enter property value etc. docker-elk_metricbeat_1"></b-input>
    </b-field>
    <b-field label="Number"
        type="is-success"
        >
        <b-input v-model="logNumber" maxlength="30" placeholder="Enter number of logs"></b-input>
    </b-field>
    <b-button @click="fetchSomething">Search</b-button>
    </div>
    <h2> Logs </h2>
    <ul>
        <li class="logs" v-for=" log,index in logs" 
         :key="index">
           <p>Log No #{{index + 1}}</p>
            <div class="modal-stuff">
              {{log}}
            </div>
        </li>
    </ul>
   
  </section>
</template>

<script>

  export default {
      data() {
          return {
              logs: '',
              logKey: 'container.name',
              logValue: 'docker-elk_metricbeat_1',
              logNumber: 0,
              isCardModalActive: false,
              currentLog: '',
              logsTry: ' '
          }

      },
  methods: {
    async copyURL(mytext) {
    try {
      await navigator.clipboard.writeText(mytext);
      alert('Copied');
    } catch($e) {
      alert('Cannot copy');
    }
  },
    setCurrentLog(){
        this.currentLog = this.logs[index];
    },
    async fetchSomething() {
        console.log(this);
        const ip = await this.$axios.$post('http://192.168.2.101:8080/get',{
            indexName :this.logKey,
            indexValue: this.logValue,
            logNumber: this.logNumber
        }).then((response)=> {
            console.log(response);
            // /"([^"]+)":/g
            // let thiefes  =  new RegExp("\"(\\w+)\":"); 
         
                // var input = "<u>ragu</u>";
                //   var regex = @"<.*?\>";
                //      var  output = Regex.Replace(input, regex, string.empty);
            // let ogo = JSON.stringify(response[0]);
            // this.logs = JSON.parse(ogo);
            // console.log(this.shit);
            this.logs = response;
            this.logsTry = this.logs[0].replace(/"(\w+)"\s*:/g, '$1:');
            console.log(this.logsTry);
            // indexName :"container.name",
            // indexValue: "docker-elk_metricbeat_1"
        });
    },
  }
}

</script>
<style >
    body{
        background-color: black;
    }
    
    .logs {
        height: 300px;
        overflow: scroll;
        display: flex;
        
    }
    li {
        margin-bottom: 2rem;
    }
    p {
        border: 1px solid white;
        color: white;

    }
    .label {
        color: white;
    }
    h2{ 
        font-size: larger;
        color: white;
        margin-bottom: 2rem;
    }
    .search {
        margin-top: 2rem;
        margin-bottom: 2rem;
        padding: 1rem;
        border: 2px solid gainsboro;
    }
    .modal-content {
        max-width: 1000px;
    }
    .modal-stuff {
        color: white;
    }
    .logBUttons {
        display: flex;
        flex-direction: column;
        height: 100%;
    }
    .button {
        height: 50%;
        border: 1px solid black;
    }
</style>