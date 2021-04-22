<template>
  <div>
    
    <header class="cabecalho">
      <Logo />
      <h1 class="leads_title">Listagem de Leads</h1>
    </header>

    <main class="conteiner">
      <button @click="filterLeads('')"> Sem filtro </button>

     <h3>Filtre pelo nome do lead:</h3>
      <label v-for="(filter) in catNome" :for="filter" :key="filter.id">
        {{filter}}
        <input :id="nome" name="escolha" type="radio" :value="filter"  @change="filterLeads($event)"/>
      </label>
      <br/><br/>

      <h3>Filtre pela categoria nome da empresa:</h3>
      <label v-for="(filter) in cat" :for="filter" :key="filter.name">
        {{filter}}
        <input :id="categoria" type="radio" name="escolha" :value="filter" @change="filterLeads($event)"/>
      </label>



      <ul class="content">
        <li  v-for="lead in filteredLeads" :key="lead.id">
          <h3>{{lead.name}}</h3>
          <div>{{lead.email}}</div><br />
          <div>{{lead.phone}}</div><br />
          <div>{{lead.website}}</div><br />
        </li>
      </ul>

    </main>

    <aside>

    </aside>

    <footer>

    </footer>
    
    
    
    
  </div>
</template>

<script>
export default {
  data() {
    return {
      nome: 'nome',
      categoria: 'categoria',
      filteredLeads: [],
      leads: []
    }
  },

  computed: {
    catNome() {
      return Array.from(new Set(this.leads.map(e => e.name)))
    },
    cat() {
      return Array.from(new Set(this.leads.map(e => e.company.bs)))
    }
  },

  methods:{
    filterLeads(event){
      if(event === ''){
        this.filteredLeads = this.leads

      }else if(event.target.id === 'nome'){
        this.filteredLeads = this.leads.filter(e =>{
          return e.name === event.target.value
        })

      }else if (event.target.id === 'categoria'){
        this.filteredLeads = this.leads.filter(e =>{
          return e.company.bs === event.target.value

        })
      }
    },

  },

  
  
  async mounted() { 
    
    this.leads = await fetch('https://jsonplaceholder.typicode.com/users', {mode: 'no-cors'}).then(res =>
      res.json()
    )
    
    this.filteredLeads = this.leads
  },

  fetchOnServer: false
}
</script>

<style lang="scss" scoped="true">

* {margin: 0; padding: 0; list-style: none; text-decoration: none; }

.conteiner{margin: 20px 2%; background-color: yellow;}

.leads_title{text-align: center; margin: 20px 2%; }  

.cabecalho{ width: 100%; height: 80px; display: block; padding: 20px 6%; background-color: violet;}

</style>
