<template>
  <div id="login" class="col-12 row">
        <h1>Acceder</h1>
        <form @submit="onSubmit">
          <div :class="{'error': isError, 'd-none': !isError}">El nombre de usuario o contraseña es incorrecto.</div>
          <div class="form-group">
            <label for="email">E-Mail: </label>
            <input class="form-control" type="text" name="email" id="email">
          </div>  
          <div class="form-group">
            <label for="password">Password: </label>
            <input class="form-control" type="password" name="password" id="password">
          </div>
          <button class="btn">Ingresar</button>
        </form>
  </div>
</template>

<script>
export default {
  name: 'LoginPage',
  components: {
  },
  props:{
    user: Array,
  },        
  data() {
      return{
        rts: false,
        isError: false
      }
  },
  methods:{
    onSubmit(event){
      event.preventDefault();
      
      const formD = new FormData(event.currentTarget);
 
      this.user.map((item) =>{
          if (formD.get("email") === item.email && formD.get("password") === item.password) {
              this.rts = true;
              this.isError = false;
          } else {
              this.rts = false;
          }

      })

      this.rts? this.$emit('LogIn'): this.isError = true;
    }
  },
  computed:{
  }     
}
</script>

<style scoped>
  #login{
    width: 800px;
    margin: 0 auto 40px;
    background-color: #fff;
    border-radius: 10px;
    padding: 50px 40px;
    border: 1px solid #ccc;
  }
  form{
    max-width: 400px;
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
  }

</style>
