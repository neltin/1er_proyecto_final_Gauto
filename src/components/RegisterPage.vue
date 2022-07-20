<template>
  <div id="register" class="col-12 row">
        <h1>Registro</h1>
        <form @submit="onSubmit">
          <div :class="{'error': isError, 'd-none': !isError}">{{error}}</div>
          <div class="form-group col-12 row">
            <div class="col-6">
                <label for="nombre">Nombre: </label>
                <input type="text" class="form-control" name="nombre" id="nombre">            
            </div>
            <div class="col-6">
              <label for="apellido">Apellido: </label>
              <input type="text" class="form-control" name="apellido" id="apellido">   
            </div>        
          </div>
          <div class="form-group col-12 row">
            <div class="col-6">
              <label for="email">E-Mail: </label>
              <input type="text" class="form-control" name="email" id="email">
              <span class="obli">*Obligatorio</span>
            </div>
            <div class="col-6">
              <label for="password">Password: </label>
              <input type="password" class="form-control" name="password" id="password">
              <span class="obli">*Obligatorio</span>
            </div>
          </div>
          <button class="btn">Registrarse</button>
        </form>        
  </div>
</template>

<script>
export default {
    name: 'RegisterPage',
    components: {
    },
    props:{
      user: Array,
    },
    data() {
        return{
          dataRegister:[],
          isError: false,
          error: ""
        }
    },
    methods: {
      onSubmit(event){
        event.preventDefault();
        const formD = new FormData(event.currentTarget);

        this.dataRegister = {"nombre":formD.get("nombre"), "apellido":formD.get("apellido"), "email":formD.get("email"), "password":formD.get("password")};

        this.user.map((item) =>{
            if (formD.get("email") === item.email || formD.get("email") == "" || formD.get("password") == "") {
              //Campo e-mail repetido
                if(formD.get("email") === item.email ){
                  this.rts = false;
                  this.error= "El usuario ya existe.";
                }
                //Campo obligatorio
                if(formD.get("email") == "" || formD.get("password") == ""){
                  this.rts = false;
                  this.error= "Falta campos obligatorios por completar";
                }
            } else {
                this.rts = true;
                this.isError = false;
            }
        })

        this.rts? this.$emit('Register', this.dataRegister) : this.isError = true;
      },
    }    
}
</script>

<style scoped>
#register{
    width: 800px;
    margin: 0 auto 40px;
    background-color: #fff;
    border-radius: 10px;
    padding: 50px 40px;
    border: 1px solid #ccc;

}
form{
  max-width: 700px;
  margin: 0 auto;
}
.form-group{
  margin-bottom: 30px;

}
label{
  margin-bottom:20px;
  float:left;
}
.btn{
  background-color: #ED8B7F;
  color:#fff;
}
.btn:hover{
  color:#fff;
}

.error{
  margin: 0 0 20px;
  color:red;
  font-weight: 600;
  font-size: 15px;
  text-align: center;
}
.obli{
  color:red;
  font-size: 12px;
}
</style>