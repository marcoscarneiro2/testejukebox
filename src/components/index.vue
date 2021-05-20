<template>
<v-card
  width="70%"
  class="mx-auto my-8"
  >
  <form @submit.prevent="AdicionarUsuario" method="post">
    <!----------------------------->
    <!--------Campo de nome-------->
    <!----------------------------->
    <v-col
          cols="12"
          sm="12"
        >
    <v-text-field
      v-model="user.name"
      :error-messages="nameErrors"
      :counter="20"
      label="Nome"
      required
      @input="$v.name.$touch()"
      @blur="$v.name.$touch()"
    ></v-text-field>
    </v-col>
    <!----------------------------->
    <!-----Campo de sobrenome------>
    <!----------------------------->
      <v-col
          cols="12"
          sm="12"
        >
     <v-text-field
      v-model="user.Sname"
      :error-messages="SnameErrors"
      :counter="20"
      label="Sobrenome"
      required
      @input="$v.Sname.$touch()"
      @blur="$v.Sname.$touch()"
    ></v-text-field>
      </v-col>
    <!----------------------------->
    <!-----Campo de telefone------->
    <!----------------------------->
      <v-col
          cols="12"
          sm="12"
        >
     <v-text-field
      v-model="user.tel"
      :counter="12"
      label="Telefone"
    ></v-text-field>
      </v-col>
    <!----------------------------->
    <!--------Campo de email------->
    <!----------------------------->
      <v-col
          cols="12"
          sm="12"
        >
    <v-text-field
      v-model="user.email"
      :error-messages="emailErrors"
      label="E-mail"
      required
      @input="$v.email.$touch()"
      @blur="$v.email.$touch()"
    ></v-text-field>
      </v-col>
    <!----------------------------->
    <!------chekbox cpf/cnpj------->
    <!----------------------------->
    <v-checkbox
      v-model="checkbox"
      label="Pessoa Jurídica ?"
      required
      @change="$v.checkbox.$touch()"
      @blur="$v.checkbox.$touch()"
    ></v-checkbox>
    <!----------------------------->
    <!--------Campo de cpf--------->
    <!----------------------------->
      <v-col
          cols="12"
          sm="12"
        >
      <v-text-field
      v-if="!checkbox"
      v-model="user.cnpjcpf"
      :counter="11"
      label="CPF"
      outlined
    ></v-text-field>
      </v-col>
    <!----------------------------->
    <!--------Campo de cnpj-------->
    <!----------------------------->
      <v-col
          cols="12"
          sm="12"
        >
      <v-text-field
      v-if="checkbox"
      v-model="user.cnpjcpf"
      :counter="14"
      label="CNPJ"
      outlined
      ></v-text-field>
      </v-col> 

      <v-col
          cols="6"
          sm="10"
        >
    <!----------------------------->
    <!------Botão de cadastrar----->
    <!----------------------------->
    <v-btn
      class="mr-4"
      @click="submit"
      v-on:click.prevent="salvarUsuario(user)"
     
    >
      Cadastrar
    </v-btn>
    <!----------------------------->
    <!----Botão de limpar campos--->
    <!----------------------------->
    <v-btn @click="clear">
      Limpar
    </v-btn>
        </v-col>
  </form>
    <!----------------------------->
    <!------Inicio da Tabela------->
    <!----------------------------->
  <v-simple-table
  fixed-header
  height="200px"
  >
    <template v-slot:default>
    <!----------------------------->
    <!------Corpo da Tabela-------->
    <!----------------------------->
      <thead>
        <tr>
          <th class="text-left">
            Nome completo
          </th>
          <th class="text-left">
            E-mail
          </th>
          <th class="text-left">
            Telefone
          </th>
          <th class="text-left">
            Cpf/Cnpj
          </th>
        </tr>
      </thead>
    <!------------------------------------->
    <!------------Linhas da Tabela--------->
    <!------------------------------------->
      <tbody>
        <tr
          v-for="user in user"
          :key="user.name"
        >
          <td>{{ user.name }}</td>
          <td>{{ user.email }}</td>
          <td>{{ user.telefone }}</td>
          <td>{{ user.cpfcnpj }}</td>

           <v-btn
             class="mx-2"
             fab
             dark
             small
             color="cyan"
            >
            <v-icon dark>
             mdi-pencil
            </v-icon>
           </v-btn>
            
           <v-btn
             class="mx-2"
             fab
             dark
             small
             color="red"
            >
            <v-icon dark>
            mdi-cancel
            </v-icon>
           </v-btn>
        </tr>
      </tbody>
    </template>
  </v-simple-table>
   </v-card>  
   
</template>

  
<script>
  import { validationMixin } from 'vuelidate'
  import { required, maxLength, email } from 'vuelidate/lib/validators'

  export default {
    mixins: [validationMixin],

    validations: {
      name: { required, maxLength: maxLength(10) },
      Sname: { required, maxLength: maxLength(10) },
      email: { required, email },
      checkbox: {
        checked (val) {
          return val
        },
      },
    },
     data: () => ({
      users:[],
      user:{
      name: '',
      Sname:'',
      email: '',
      tel:'',
      cnpjcpf:''
       },
       checkbox: false
    }),
    computed: {
      nameErrors () {
        const errors = []
        if (!this.$v.name.$dirty) return errors
        !this.$v.name.maxLength && errors.push('O nome deve ter no máximo 10 caracteres')
        !this.$v.name.required && errors.push('O nome é obrigatório.')
        return errors
      },
        SnameErrors () {
        const errors = []
        if (!this.$v.Sname.$dirty) return errors
        !this.$v.Sname.maxLength && errors.push('O nome deve ter no máximo 10 caracteres')
        !this.$v.Sname.required && errors.push('O sobrenome é obrigatório.')
        return errors
      },
      emailErrors () {
        const errors = []
        if (!this.$v.email.$dirty) return errors
        !this.$v.email.email && errors.push('Deve ser um e-mail válido')
        !this.$v.email.required && errors.push('E-mail é obrigatório')
        return errors
      },
    },

    methods: {
      submit () {
       this.$v.$touch()
      },
      clear () {
        this.$v.$reset()
        this.user.name = ''
        this.user.email = ''
        this.user.Sname = ''
        this.user.tel = ''
        this.user.cnpjcpf = ''
        this.checkbox = false
      },
      created(){
        this.users = JSON.parse(localStorage.getItem('usersApp'));
      },
      salvarUsuario(user){
        let users = localStorage.getItem('usersApp');

        if(users){
          //Atualizo o usuario 
          users = JSON.parse(users);
          //user.push=[(users)];
        }else{
          //crio o usuario 
          users = [user];
        }
        //atualizar o local storage
        localStorage.setItem('usersApp',JSON.stringify(user))
      }
    },
  }
  
</script>