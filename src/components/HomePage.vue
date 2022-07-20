<template>
  <div id="home" class="container-fluid row">
    <div v-show="!accesso" class="col-12">
        <div class="marco">
            <img alt="Vue logo" src="../assets/logo.png" class="logo" />
        </div>    
        <ul class="nav-center">
            <li class="nav-item"><a @click="chanceLog" :class="navActive1">Acceder</a></li>
            <li class="nav-item"><a @click="chanceLog" :class="navActive2">Registrarse</a></li>
        </ul>

        <loginPage v-show="!chancePage" :user="user" @LogIn="log" />            
        <RegisterPage v-show="chancePage" :user="user" @Register="Register" />     
    </div>
    <div v-show="accesso">
        <nav class="navbar flex-column flex-md-row navbar-header">
            <span class="navbar-brand">
                <img alt="Vue logo" src="../assets/logo.png" width="36" height="36" class="logo-header d-block" /> 
            </span>
            <ul class="navbar-nav bd-navbar-nav flex-row">
                <li class="nav-item">
                    <a @click="log" class="nav-link">Cerrar sesión</a>
                </li>
                <li class="nav-item">
                    <a @click="pageCart" class="nav-link">{{carrito}}</a>
                </li>
            </ul>
        </nav>
        <ProductsPage :lista="lista" :chancePageCart="chancePageCart" />
    </div> 
  </div>
</template>

<script>
import LoginPage from './LoginPage.vue';
import RegisterPage from './RegisterPage.vue';
import ProductsPage from './ProductsPage.vue';

export default {
    name: 'HomePage',
    components: {
        LoginPage, RegisterPage, ProductsPage
    },
    data() {
        return{
            user:[
                {nombre:"Admin", apellido:"User", email:"user@algo.com.ar", password:"1234"}
            ],
            lista:[
                {id: 1, titulo: "Zapatillas adidas Galaxy 5", descripcion: "Las Zapatillas adidas Galaxy 5 presentan un diseño moderno y urbano, ideal para los runners que aman la moda. Su exterior de malla aporta transpirabilidad y resistencia, mientras que la suela de caucho aporta tracción en diversas superficies. Su plantilla OrthoLite® se combina con una mediasuela con amortiguación Cloudfoam para proporcionar un confort superior y dejarte fluir en cada carrera.", precio: "14699", imagen: "Galaxy_5.jpg"},

                {id: 2, titulo: "Zapatillas Puma Flyer Flex", descripcion: "Las Zapatillas Puma Flyer Flex son cómodas y frescas, ideales para acompañarte en todos tus entrenamientos. Su plataforma Flyer Runner aporta flexibilidad a cada pisada para que se adapten al pie y garanticen un confort pleno, mientras lucís un estilo moderno y urbano.", precio: "13699", imagen: "Puma_Flyer_Flex.jpg"},

                {id: 3, titulo: "Zapatillas Fila Revelation", descripcion: "El modelo de zapatillas Fila Revelation es un producto pensado para los corredores que se están iniciando en el running y que buscan completa comodidad. Tanto para sus jornadas de corridas, como para el día a día. Están confeccionadas en su parte superior con piel de poliester que garantiza durabilidad. Además, la suela de goma con la que están fabricadas, promete comodidad, liviandad y ligereza en cada paso. Su diseño clásico, completa tu estilo para todo el día. Por eso, este es tu calzado perfecto.", precio: "8799", imagen: "Fila_Revelation.jpg"},

                {id: 4, titulo: "Zapatillas Nike Downshifter 11", descripcion: "Las Zapatillas Nike Downshifter 11 son perfectas para tus días de running, la malla superior te brinda comodidad transpirable, con ranuras flexibles en la suela te da mejor tracción, reacción, y amortiguación en cada pisada, además el logotipo swoosh metálico le añade un toque deportivo y original.", precio: 16.999, imagen: "Nike_Downshifter_11.jpg"},

                {id: 5, titulo: "Zapatillas Nike Run Swift 2", descripcion: "Si buscás un modelo perfecto para levantar pesas o correr, las Zapatillas Nike Run Swift 2 son ideales para tus entrenamientos ya que te brindan mayor velocidad, tracción y estabilidad. Su capellada en malla te brinda transpirabilidad y frescura. Te dan mejor ajuste y seguridad en los momentos de más esfuerzo; la suela hecha de goma maciza te dará mejor agarre y confort en los diferentes terrenos a donde vayas.", precio: "16499", imagen: "Nike_Run_Swift_2.jpg"},

                {id: 6, titulo: "Zapatillas Asics Gel-Backhand Standard", descripcion: "Las zapatillas Asics Gel-Backhand Standard están pensadas par que aproveches al máximo tus kilómetros durante la corrida. Es un modelo liviano y duradero que garantiza comodidad y resistencia para que puedas alcanzar tus metas e incluso superarlas. Están confeccionadas con material sintético y mesh y su entresuela con tecnología GEL proporciona un altísima amortiguación, reduciendo los impactos, así tendrás comodidad asegurada en todo momento. La suela de caucho para canchas rápidas cuenta con alta calidad y ofrece máxima durabilidad. Tienen un estilo elegante y sofisticado, para que no dejes de ser vos en ningún momento.", precio: "22099", imagen: "Asics_Gel_Backhand_Standard.jpg"},
            ],            
            chancePage: false,
            accesso: false, //cambiar a false
            chancePageCart: false,
            carrito: "Carrito de compra"
        }
    },
    methods: {
        log(){
            this.accesso = !this.accesso;
        },
        chanceLog(){
            this.chancePage = !this.chancePage;
        },
        Register(dataRegister){
            this.user.push(dataRegister);
            this.chancePage = !this.chancePage;
        },
        pageCart(){
            this.chancePageCart = !this.chancePageCart;
            this.chancePageCart ? this.carrito = "Cerrar carrito" : this.carrito = "Carrito de compra";   
        } 
    },
    computed:{
        navActive1(){
            return this.chancePage ? "nav-link" : "nav-link active";
        },
        navActive2(){
            return this.chancePage ? "nav-link active" : "nav-link";
        }
    }     
}
</script>

<style>
    .nav-center{
        margin: 0 auto;
        background-color: #fff;
        list-style: none;
        padding: 0;
            width: 342px;
        overflow: hidden;
        display: block;
        border-radius: 10px 10px 0 0;
        border-top: 1px solid #ccc;
        border-left: 1px solid #ccc;
        border-right: 1px solid #ccc;       
    }

    .nav-center .nav-item{
        width: 170px;
        padding: 0;
        float: left;
    }
    .nav-center .nav-item .nav-link{
        color:#585858;
        padding: 10px 20px;
        background-color: #ddd;
        text-align: center;
        cursor: pointer;
    }
    .nav-center .nav-item:nth-child(1) .nav-link{
        border-right: 1px solid #bbb;
    }
    .nav-center .nav-item .nav-link.active{
        color:#ED8B7F;
        background-color: #fff;
    }

    .navbar-header{
        background-color: #fff;
        padding: 20px;
        margin-bottom: 40px;

        border-radius: 0 0 10px 10px;
        border-bottom: 1px solid #ccc;
        border-left: 1px solid #ccc;
        border-right: 1px solid #ccc;         
    }
    .navbar-header .nav-item .nav-link{
        color:#585858;
        margin-right: 25px;
        cursor: pointer;
    }

    .navbar-header .nav-item .nav-link:hover{
        color:#ED8B7F;
    }
</style>
