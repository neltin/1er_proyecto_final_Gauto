<template>
  <div id="product">
        <a class="volver" @click="volver">Volver a productos</a>
        <div class="card">
            <div class="row">
                <div class="col-md-6">
                    <img :src="require(`@/assets/${detail.imagen}`)" :alt="detail.titulo" />
                </div>
                <div class="col-md-6">
                    <h3 class="titulo">{{detail.titulo}}</h3> 
                    <p class="price">${{detail.precio}}</p>   
                    <p class="descripcion">{{detail.descripcion}}</p>
                    <button class="btn btn-primary" @click="addCart(detail.id)">Agregar al carrito</button>     
                </div>    
            </div>
        </div>
  </div>
</template>

<script>
export default {
    name: 'ProductPage',
    components: {
    },
    props: {
      detail: Object
    },    
    data() {
        return{
            listCart:[]
        }
    },
    methods: {
        volver(){
            this.$emit('changePage');
        },
        //Agregar lista de carritos y localStorege        
        addCart(id){
            const cart = localStorage.getItem('Carts');
            //Si hay guardado en el localStorage cargar lista de compra            
            if(cart){ 
                this.listCart = JSON.parse(cart)
            }  

            const Index = this.listCart.findIndex((obj => obj.id == id));

            if(Index >= 0){
                this.listCart[Index].cant += 1;
                this.listCart[Index].subTotal = this.listCart[Index].cant * this.detail.precio;                
            }else{               
                const pdto = {id: this.detail.id, producto: this.detail, cant: 1, subTotal: this.detail.precio};
                this.listCart = [...this.listCart, pdto];
            }
 
            const parsed = JSON.stringify(this.listCart);
            localStorage.setItem('Carts', parsed);     
        }    
    }     
}
</script>

<style scoped>
    .card{
        padding: 60px;
        margin-bottom: 60px;
    }
    .card img{
        width:90%;
        max-width: 400px;
        height: auto;
        display: block;
        float: left;
    }

    .card .titulo{
        font-size: 35px;
        color:#000;
        margin-bottom: 20px;
    }
    .card .price{
        font-size: 30px;
        font-weight: 600;
        color:#000;
        margin-bottom:20px;
    }
    .card .description{
        font-size: 20px;
        color:#585858;
    }

    .card .btn.btn-primary{
        background-color: #ED8B7F;
        border: none;
        margin-top:20px;
    }

    .volver{
        margin: 20px auto 40px;
        text-align: center;
        display: block;
        color: #000;    
        cursor: pointer;
    }
    .volver:hover{
        color: #ED8B7F;
    }
</style>