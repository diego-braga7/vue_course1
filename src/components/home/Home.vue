
<template>
  <div>
    <h1 class="centralizado">{{ titulo }}</h1>
    <input type="search" class="filtro" v-on:input="filtro = $event.target.value" placeholder="Filtro por título" >
    <ul class="lista-fotos">
      <li class="lista-fotos-item" v-for="foto of fotosComFiltros">
        <meu-painel :titulo="foto.titulo">
            <imagem-responsiva :src="foto.url" :titulo="foto.titulo"></imagem-responsiva>
            <meu-botao tipo="button" rotulo="remover" @botaoAtivado="remove(foto)" :confirmacao="true" estilo="perigo" />
        </meu-painel>
      </li>
    </ul>

  </div>
</template>

<script>
import Painel from '../shared/painel/Painel.vue';
import ImagemResponsiva from '../shared/imagem-responsiva/ImagemResponsiva.vue';
import Botao from '../shared/botao/Botao.vue';

export default {

    components: {
      'meu-painel' : Painel,
      'imagem-responsiva' : ImagemResponsiva,
      'meu-botao': Botao
    },

  data() {
    return {
      titulo: "alura pic",
      subtitulo: "Sub Titulo",
      fotos: [],
      filtro: ''
    };
  },
  methods: {

    remove(foto){
        alert('quer remover:'+foto.titulo);
    }
  },
  computed: {
    fotosComFiltros(){
      if(this.filtro){
        let exp = new RegExp(this.filtro.trim(), 'i');
        return this.fotos.filter(foto => exp.test(foto.titulo))
      }else{
        return this.fotos;
      }
    }
  },
  created(){
    this.fotos = this.$http.get('http://localhost:3000/v1/fotos')
    .then(res => res.json())
    .then(fotos => this.fotos = fotos, err => console.log(err));
    
  }
};
</script >

<style >

 .centralizado{
   text-align: center;
 }

 .lista-fotos{
   list-style: none;
 }

 .lista-fotos .lista-fotos-item{
   display: inline-block;
 }
 
  .filtro{
    display: block;
    width: 100%;
  }
</style>
