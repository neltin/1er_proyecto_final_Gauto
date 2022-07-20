<template>
  <div id="cart">
        <h1>Carrito de compras</h1>
        <div class="container-item card" v-for="(data, key) in listCart" :key="key">
            <div class="row">
                <div class="col-6 row">
                    <div class="col-3">
                        <img :src="require(`@/assets/${data.producto.imagen}`)" :alt="data.producto.titulo" />
                    </div>
                    <div class="col-9">
                    <h3 class="titulo">{{data.producto.titulo}}</h3>
                    </div>
                </div>
                <div class="col-2 row">
                    <div class="restar">
                        <button @click="restar(data.id)"> - </button>
                    </div>
                    <div class="contador">                
                        <input type="text" v-model="data.cant" readonly>
                    </div>
                    <div class="sumar">                
                        <button @click="sumar(data.id)"> + </button>
                    </div>
                </div>
                <div class="col-4 row">
                    <div class="col-6">
                    <p>Precio</p>    
                    <p class="price">${{data.producto.precio}}</p>
                    </div>
                    <div class="col-6">
                        <p>Sub-total</p>
                        <p class="price">${{data.subTotal}}</p>
                    </div>                
                </div>
                <div class="col-12">
                    <hr />
                    <a class="eliminar" @click="eliminar(data.id)"> Eliminar </a>
                </div>
            </div>            
        </div>
        <div class="col-12 card">
           
            <h4 class="total">Total: ${{total}}</h4>
        </div>        
  </div>
</template>

<script>

export default {
    name: 'CartPage',
    components: {
    },
    data(){
        return{
            listCart: []
        }
    },
    mounted() {
        if (localStorage.getItem('Carts')) {
            this.listCart = JSON.parse(localStorage.getItem('Carts'));
        }
    },
    methods: {        
        sumar(id){
            const Index = this.listCart.findIndex((obj => obj.id == id));
            

            if(Index >= 0){
                this.listCart[Index].cant += +1;
                this.listCart[Index].subTotal = this.listCart[Index].cant * this.listCart[Index].producto.precio;
                this.getLocalStorage(); 
            }
        },
        restar(id){
            const Index = this.listCart.findIndex((obj => obj.id == id));

            if(Index >= 0){
                this.listCart[Index].cant += -1;
                this.listCart[Index].subTotal = this.listCart[Index].cant * this.listCart[Index].producto.precio;

                if(this.listCart[Index].cant <= 0){
                    this.listCart.splice(Index , 1);
                }

                this.getLocalStorage(); 
            }
        },
        eliminar(id){
            const Index = this.listCart.findIndex((obj => obj.id == id));
            this.listCart.splice(Index , 1);

            this.getLocalStorage();
        },
        getLocalStorage(){
            const parsed = JSON.stringify(this.listCart);
            localStorage.setItem('Carts', parsed);
        },        
    },
    computed:{
        total(){
            let tot = 0;
            this.listCart.map(i => tot+= parseInt(i.subTotal))
            return tot;

        }
    }    
    
}
</script>

<style scoped>
.card{
    margin-bottom:20px;
    min-height: 100px;
    padding: 40px 40px 20px;
}
.card img{
    width:100%;
    max-width: 70px;
    height: auto;
    display: block;
    float: left;
}

.card .titulo{
    font-size: 18px;
    color:#000;
    margin-top:15px;
    padding: 10px 20px;
    display: block;
    float: left;    
}
.card .price{
    font-size: 22px;
    font-weight: 600;
    color:#000;
    margin-bottom:20px;
}
.card .description{
    font-size: 15px;
    color:#585858;
}
.card .total{
    text-align: right;
    margin-top: 20px;
    margin-right: 20px;
    font-weight: 600;
}
.contador input{
    text-align: center;
    max-width: 30px;
    max-height: 30px;
    font-size: 15px;
    font-weight: 600;
    padding: 5px;
    margin:0 auto;
    display: block;
    border:none;
    border-left: 1px solid #bbb;
    border-right: 1px solid #bbb;    
}
.restar button, .sumar button{
    width: 30px;
    height: 30px;
    border: none;
    font-size: 15px;
    display: flex;
    align-items: center;
    justify-content: center;
    margin: 0 auto;
    text-align: center; 
    border: 1px solid #bbb;          
}

.restar button:hover, .sumar button:hover{
    background-color: #ED8B7F;
}
.eliminar{
    color:#000;
    text-decoration: none;
}

.eliminar:hover{
    color:#ED8B7F;
}

</style>
