<template>
   <div>
    <v-card>

      <v-card-title flat color="white">
        <v-layout row>
          <v-flex xs2>
            <v-btn color="primary">新增品牌</v-btn>
          </v-flex>
          <v-spacer/>
          <v-flex xs4>
            <v-text-field label="搜索" hide-details append-icon="search" v-model="key"></v-text-field>
          </v-flex>
        </v-layout>
      </v-card-title>

      <v-data-table
        :headers="headers"
        :items="brands"
        :pagination.sync="pagination"
        :total-items="totalBrands"
        :loading="loading"
        class="elevation-1"
      >

        <template slot="items" slot-scope="props">
          <td class="text-xs-center">{{ props.item.id }}</td>
          <td class="text-xs-center">{{ props.item.name }}</td>
          <td class="text-xs-center">
            <img v-if="props.item.image" :src="props.item.image" width="130" height="40">
            <span v-else>无</span>
          </td>
          <td class="text-xs-center">{{ props.item.letter }}</td>
          <td class="text-xs-center">
            <v-btn text icon color="info" small>
            <v-icon>edit</v-icon>
          </v-btn>
            <v-btn text icon color="error" small>
              <v-icon>delete</v-icon>
            </v-btn>
          </td>
         <!-- <td class="justify-center layout px-0">
            <v-btn icon @click="editBrand(props.item)">
              <i class="el-icon-edit"/>
            </v-btn>
            <v-btn icon @click="deleteBrand(props.item)">
              <i class="el-icon-delete"/>
            </v-btn>
          </td>-->
        </template>
      </v-data-table>

    </v-card>
   </div>
</template>

<script>
    export default {
        name: "MyBrand",
        data(){
          return {
            headers:[
              {text: 'id', align: 'center', value: 'id'},
              {text: '名称', align: 'center', value: 'name', sortable: false},
              {text: 'LOGO', align: 'center', value: 'image', sortable: false},
              {text: '首字母', align: 'center', value: 'letter'},
              {text: '操作', align: 'center'}
            ],
            brands:[],
            pagination:{},
            totalBrands:0,
            loading:false,
            key:"" //搜索的字符串
          }
        },

        //钩子函数
        created() {
          /*this.brands=[
            {
              "id": 2032,
              "name": "OPPO",
              "image": "http://img10.360buyimg.com/popshop/jfs/t2119/133/2264148064/4303/b8ab3755/56b2f385N8e4eb051.jpg",
              "letter": "O",
            },
            {
              "id": 2033,
              "name": "飞利浦（PHILIPS）",
              "image": "http://img12.360buyimg.com/popshop/jfs/t18361/122/1318410299/1870/36fe70c9/5ac43a4dNa44a0ce0.jpg",
              "letter": "F",
            },
            {
              "id": 2034,
              "name": "华为（HUAWEI）",
              "image": "http://img10.360buyimg.com/popshop/jfs/t5662/36/8888655583/7806/1c629c01/598033b4Nd6055897.jpg",
              "letter": "H",
            },
            {
              "id": 2036,
              "name": "酷派（Coolpad）",
              "image": "http://img10.360buyimg.com/popshop/jfs/t2521/347/883897149/3732/91c917ec/5670cf96Ncffa2ae6.jpg",
              "letter": "K",
            },
            {
              "id": 2037,
              "name": "魅族（MEIZU）",
              "image": "http://img13.360buyimg.com/popshop/jfs/t3511/131/31887105/4943/48f83fa9/57fdf4b8N6e95624d.jpg",
              "letter": "M",
            }
          ];
          this.totalBrands = 15;*/
          this.loadBrands();
        },

      watch:{
        key(){
          this.pagination.page = 1;
          this.loadBrands();
        },
        pagination:{
          deep:true,
          handler(){
            this.loadBrands();
          }
        }
      },

      methods:{
        loadBrands(){
          this.loading = true;
          this.$http.get("/item/brand/page",{
            params:{
              key: this.key,
              page: this.pagination.page,
              rowsPerPage: this.pagination.rowsPerPage,
              sortBy: this.pagination.sortBy,
              descending: this.pagination.descending
            }
          }).then(resp =>{
            //后台返回给前端的
            this.brands = resp.data.items;
            this.totalBrands = resp.data.total;
          });
          this.loading = false;
        }
      },

    }
</script>

<style scoped>

</style>
