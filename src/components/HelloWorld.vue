<template>
  <div class="hello">
    <h1>{{ msg }}</h1>

    <b-alert variant="danger"
             dismissible
             :show="showDismissibleAlert"
             @dismissed="showDismissibleAlert=false">
      {{ alertGetProduct }}
    </b-alert>

    <b-table  hover :fields="filedsProductos" :index="2" :items="products">

      <template slot="i18n" :itemid="pt_PT" slot-scope="data">
            {{data.value[0].pt_PT.description}}
      </template>
      <template slot="show_details" slot-scope="row">

        <b-button size="sm" @click.stop="row.toggleDetails" class="mr-2">
          {{ row.detailsShowing ? 'Hide' : 'Show'}} Details
        </b-button>

      </template>
      <template slot="row-details" slot-scope="row">
        <b-card>
          <b-row class="mb-2">
            <b-col sm="3" class="text-sm-right"><b>Name:</b></b-col>
            <b-col>{{ row.item.i18n[0].pt_PT.name }}</b-col>
          </b-row>
          <b-row class="mb-2">
            <b-col sm="3" class="text-sm-right"><b>Description:</b></b-col>
            <b-col>{{ row.item.i18n[0].pt_PT.description }}</b-col>
          </b-row>
          <b-row class="mb-2">
            <b-col sm="3" class="text-sm-right"><b>Image:</b></b-col>
            <b-col>{{ row.item.url_video }}
              <div>
              <b-img src="{ row.item.url_video }" fluid alt="Responsive image" />
            </div></b-col>
          </b-row>
          <b-button size="sm" @click="row.toggleDetails">Hide Details</b-button>
        </b-card>
      </template>

    </b-table>

    <template>
      <div>
        <b-pagination size="md" :total-rows="totalRows" v-model="currentPage" :per-page="perPage" v-on:click.native="getProducts">
        </b-pagination>
        <br>

      </div>
    </template>


  </div>
</template>

<script>
import axios from 'axios';

const instance = axios.create({
  baseURL: 'https://api.facestore.pt',

  headers: { APIToken: 'b6829b5d2950221a0a05c8be4c85a00b6b67ce8c' },
});
export default {
  name: 'HelloWorld',
  data() {
    return {
      msg: 'Teste para o BACKEND',
      products: [],
      product: [],
      meta: [],
      datas: [],
      datasAtr: [],
      productsAtr: [],
      filedsProductos: [
        { key: 'sku' },
        { key: 'titulo' },
        'show_details',
      ],
      currentPage: 1,
      totalRows: 0,
      perPage: 0,
      alertGetProduct: '',
      showDismissibleAlert: false,

    };
  },

  mounted() {
    this.getProducts();
  },
  methods: {

    getProducts() {
      instance.get(`/v1/products?page=${this.currentPage}`).then((response) => {
        this.datas = response.data;
        this.meta = this.datas.meta;
        this.products = this.datas.data;
        this.totalRows = this.meta.total;
        this.perPage = this.meta.per_page;
      }).catch((error) => {
        this.showDismissibleAlert = true;
        this.alertGetProduct = error.message;
      });
    },

  },
};
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
h1, h2 {
  font-weight: normal;
}
ul {
  list-style-type: none;
  padding: 0;
}
li {
  display: inline-block;
  margin: 0 10px;
}
a {
  color: #42b983;
}
</style>
