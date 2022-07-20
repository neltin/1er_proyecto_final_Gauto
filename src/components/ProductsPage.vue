<template>
    <div id="products" class="row">
        <div v-show="!chancePageDetail && !chancePageCart">
            <h1>Produtos</h1>
            <div class="row">
                <div v-for="(data, key) in lista" :key="key" class="col-12 col-md-6">
                    <div class="card">
                        <div class="row card-body">
                            <div class="col-4"> 
                                <img :src="require(`@/assets/${data.imagen}`)" :alt="data.titulo" /> 
                            </div>
                            <div class="col-8">
                                <h3 class="titulo">{{data.titulo}}</h3>
                                <div class="price">${{data.precio}}</div>
                                <p class="description">{{cortarTexto(data.descripcion)}}...</p>
                                <div>
                                    <button class="btn btn-primary" @click="addCart(data.id)">Agregar al carrito</button>
                                    <a class="detalle" @click="changeDetail(key)">Ver Detalle...</a>
                                </div>
                            </div>
                        </div>
                    </div>
                </div>
            </div>    
        </div>
        <div v-if="chancePageDetail && !chancePageCart">
            <ProductPage :detail="detail" @changePage="changePageProducts" />
        </div>
        <CartPage v-if="chancePageCart" @changePage="changePageCart" />             
    </div>
</template>

<script>
import ProductPage from './ProductPage.vue';
import CartPage from './CartPage.vue';

export default {
    name: 'ProductsPage',
    components: {
        ProductPage, CartPage 
    },
    props: {
      lista: Array,
      chancePageCart: Boolean 
    },    
    data() {
        return{
            chancePageDetail: false,           
            detail: {},
            listCart:[]
        }
    },
    methods: {
        cortarTexto(texto){
            return texto.slice(0,100)
        },
        //Agregar lista de carritos y localStorege
        addCart(id){
            const cart = localStorage.getItem('Carts');
            //Si hay guardado en el localStorage cargar lista de compra
            if(cart){ 
                this.listCart = JSON.parse(cart);
            }

            const [rts] = this.lista.filter((l) => ( l.id == id ?  true :  false ));

            const Index = this.listCart.findIndex((obj => obj.id == id));

            if(Index >= 0){
                this.listCart[Index].cant += 1;
                this.listCart[Index].subTotal = this.listCart[Index].cant * rts.precio;
            }else{  
                const pdto = {id: rts.id, producto: rts, cant: 1, subTotal: rts.precio};
                this.listCart = [...this.listCart, pdto];
            }
            
            const parsed = JSON.stringify(this.listCart);
            localStorage.setItem('Carts', parsed);     
        },
        //Cambios de paginas
        changeDetail(id){
            this.detail = this.lista[id];
            this.chancePageDetail = !this.chancePageDetail;           
        },
        changePageProducts(){
            this.chancePageDetail = !this.chancePageDetail;
        },
        changePageCart(){
            
            this.$emit('changeCart');
        }                       
    }    
}
</script>

<style scoped>
.card{
    margin-bottom:20px;
    min-height: 280px;
}

.card img{
    width:100%;
    max-width: 150px;
    height: auto;
}

.card .titulo{
    font-size: 22px;
    color:#000;
}
.card .price{
    font-size: 22px;
    font-weight: 600;
    color:#000;
    margin-bottom:20px ;
}
.card .description{
    font-size: 15px;
    color:#585858;
}
.card .btn.btn-primary{
    background-color: #ED8B7F;
    border: none;
}

.card .detalle{
    float: right;
    margin-right: 15px;
    margin-top: 6px;
    text-decoration: underline;
    padding: 0 10px;
    cursor: pointer;
    color: #585858;
}

ul{
    list-style: none;
}

</style>
