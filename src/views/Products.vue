<template>
  <div class="products">
    <h1 class="subtitle-1 grey--text text--darken-2">Products</h1>

    <v-container class="my-5">
      <div class="d-flex justify-space-between">
        <div>
          <span class="grey--text text--darken-1">Sort by:</span>
          <v-layout row class="mt-1 mb-4">
            <v-btn small text color="grey" @click="sortBy('name')">
              <v-icon left small>mdi-alphabet-latin</v-icon>
              <span class="caption text-lowercase">name</span>
            </v-btn>
            <v-btn small text color="grey" @click="sortBy('category')">
              <v-icon left small>mdi-shape</v-icon>
              <span class="caption text-lowercase">category</span>
            </v-btn>
            <v-btn small text color="grey" @click="sortBy('price')">
              <v-icon left small>mdi-cash-multiple</v-icon>
              <span class="caption text-lowercase">price</span>
            </v-btn>
            <v-btn small text color="grey" @click="sortBy('stock')">
              <v-icon left small>mdi-numeric</v-icon>
              <span class="caption text-lowercase">stock</span>
            </v-btn>
          </v-layout>
        </div>

        <div>
          <span class="grey--text text--darken-1">Category filter:</span>
          <v-layout row class="mt-1 mb-4">
            <v-btn small text color="grey" @click="filter = ''">
              <span class="caption text-lowercase">none</span>
            </v-btn>
            <v-btn small text color="grey" @click="filter = 'T-Shirts'">
              <span class="caption text-lowercase">t-shirts</span>
            </v-btn>
            <v-btn small text color="grey" @click="filter = 'Shirts'">
              <span class="caption text-lowercase">shirts</span>
            </v-btn>
            <v-btn small text color="grey" @click="filter = 'Pants'">
              <span class="caption text-lowercase">pants</span>
            </v-btn>
            <v-btn small text color="grey" @click="filter = 'Outers'">
              <span class="caption text-lowercase">outers</span>
            </v-btn>
            <v-btn small text color="grey" @click="filter = 'Accessories'">
              <span class="caption text-lowercase">accessories</span>
            </v-btn>
          </v-layout>
        </div>
      </div>

      <v-layout row wrap>
        <v-flex xs12 sm6 lg3 v-for="product in products" :key="product.id">
          <v-card flat class="text-xs ma-3">
            <v-carousel height="100%">
              <v-carousel-item
                v-for="(url, i) in images_url"
                :key="i"
                :src="product[url]"
                v-if="product[url] != ''"
              ></v-carousel-item>
            </v-carousel>
            <!-- <v-responsive class="">
              <v-img :src="product.image_url"></v-img>
            </v-responsive> -->
            <v-card-text>
              <div class="d-flex justify-space-between">
                <div class="subtitle-1">{{ product.name }}</div>

                <v-menu bottom offset-y>
                  <template v-slot:activator="{ on, attrs }">
                    <v-btn icon v-bind="attrs" v-on="on">
                      <v-icon>mdi-dots-vertical</v-icon>
                    </v-btn>
                  </template>
                  <v-list>
                    <v-list-item @click="() => {}">
                      <v-list-item-title>
                        <EditForm :product="product"></EditForm>
                      </v-list-item-title>
                    </v-list-item>
                    <v-list-item @click="() => {}">
                      <v-list-item-title
                        @click.stop="deleteDialog = true"
                        class="black--text text-decoration-none"
                        >Delete</v-list-item-title
                      >
                    </v-list-item>
                    <v-dialog v-model="deleteDialog" max-width="290">
                      <v-card>
                        <v-card-title class="text-h5">
                          Are you sure you want to delete
                          {{ product.name }} from products?
                        </v-card-title>

                        <v-card-text class="red--text text--darken-1">
                          Delete action cannot be undone!
                        </v-card-text>

                        <v-card-actions>
                          <v-spacer></v-spacer>

                          <v-btn text @click="deleteDialog = false">
                            No
                          </v-btn>

                          <v-btn
                            color="green darken-1"
                            text
                            @click="deleteProduct(product.id)"
                          >
                            Yes, sure
                          </v-btn>
                        </v-card-actions>
                      </v-card>
                    </v-dialog>
                  </v-list>
                </v-menu>
              </div>
              <div class="grey--text">Rp. {{ product.price }}</div>
              <div class="grey--text">Stock: {{ product.stock }} pcs</div>
            </v-card-text>
          </v-card>
        </v-flex>
      </v-layout>
    </v-container>
  </div>
</template>

<script>
import EditForm from "../components/EditForm.vue";

export default {
  name: "Products",
  components: { EditForm },
  data() {
    return {
      desc: false,
      filter: "",
      deleteDialog: false,
      images_url: [
        "image_url1",
        "image_url2",
        "image_url3",
        "image_url4",
        "image_url5"
      ]
    };
  },
  computed: {
    products() {
      if (!this.filter) return this.$store.state.products;
      else return this.$store.getters.filter(this.filter);
    }
  },
  methods: {
    deleteProduct(id) {
      this.$store.dispatch("deleteProduct", id);
      this.deleteDialog = false;
    },
    sortBy(prop) {
      console.log(this.products);
      this.desc = !this.desc;
      if (!this.desc)
        this.products.sort((a, b) => (a[prop] < b[prop] ? -1 : 1));
      else this.products.sort((a, b) => (a[prop] > b[prop] ? -1 : 1));
    }
  },
  created() {
    this.$store.dispatch("fetchData");
  }
};
</script>

<style scoped>
.pointer {
  cursor: pointer;
}
</style>
