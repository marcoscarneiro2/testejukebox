<template>
<v-card
  width="70%"
  class="mx-auto my-8"
  >
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
          v-for="item in tabela"
          :key="item.name"
        >
          <td>{{ item.name }}</td>
          <td>{{ item.email }}</td>
          <td>{{ item.telefone }}</td>
          <td>{{ item.cpfcnpj }}</td>

           <v-btn
             class="mx-2"
             fab
             dark
             small
             color="cyan"
            >
            <v-icon dark>
             mdi-pencil {{ item.editar }}
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
            mdi-cancel {{ item.cancelar }}
            </v-icon>
           </v-btn>
        </tr>
      </tbody>
    </template>
  </v-simple-table>
</v-card>
</template>

<script>
  export default {
    data () {
      return {
        tabela: [
          {
            name: '',
            email: '',
            telefone:'',
            cpfcnpj:''
            
          },
          
        ],
      }
    },
  }
</script>