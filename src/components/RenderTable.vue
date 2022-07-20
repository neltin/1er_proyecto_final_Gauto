<template>
    <div class="col-12 f-table">
            <h2>{{formato}}</h2>
            <table :class="data_class">
                <thead>
                    <tr>
                        <th :style="data_style">Titulo</th>
                        <th :style="data_style">Director</th>
                        <th :style="data_style">Genero</th>
                    </tr>
                </thead>
                <tbody>
                    <tr v-for="(data, key) in lista" :key="key"> 
                        <td>{{data.titulo}}</td>
                        <td>{{data.director}}</td>
                        <td>{{data.genero}}</td>
                    </tr>
                </tbody>
            </table>
        </div>
</template>

<script>
export default {
  name: 'RenderTable',
  props: {
    data_class: Object,
    data_style: Object,
    lista: Array,
    formato: String
  },
  methods:{
  }
}
</script>

<style scoped>
    h2{
        text-transform: capitalize;
        margin-bottom: 25px;
    }

    .f-table{
        margin-bottom: 25px;
    }
</style>