<!-- eslint-disable camelcase -->
<script>
import UserProductModalComponent from '../components/UserProductModalComponent/UserProductModalComponent.vue';

const { VITE_URL, VITE_PATH } = import.meta.env;

export default {
  components: {
    UserProductModalComponent,
  },
  data() {
    return {
      products: [],
      tempProductId: '',
    };
  },
  methods: {
    getProductsData() {
      this.$http
        .get(`${VITE_URL}/api/${VITE_PATH}/products/all`)
        .then((res) => {
          this.products = res.data.products;
        })
        .catch((err) => {
          alert(err.response.data.message);
        });
    },
    getProductDetail(id) {
      this.tempProductId = id;
    },
    addToCart(product_id, qty = 1) {
      const data = {
        product_id,
        qty,
      };
      this.$http
        .post(`${VITE_URL}/api/${VITE_PATH}/cart`, { data })
        .then((res) => {
          console.log(res.data.data);
          this.$refs.userProductModal.closeModal();
        })
        .catch((err) => {
          alert(err);
        });
    },
  },
  mounted() {
    this.getProductsData();
  },
};
</script>

<template>
  <div class="container">
    <div class="mt-4">
      <!-- 產品Modal -->
      <UserProductModalComponent
        ref="userProductModal"
        :temp-product-id="tempProductId"
        :add-to-cart="addToCart"
      ></UserProductModalComponent>
      <!-- 產品Modal -->

      <table class="table align-middle">
        <thead>
          <tr>
            <th>圖片</th>
            <th>商品名稱</th>
            <th>價格</th>
            <th></th>
          </tr>
        </thead>
        <tbody>
          <tr v-for="product in products" :key="product.id">
            <td style="width: 200px">
              <div
                style="
                  height: 100px;
                  background-size: cover;
                  background-position: center;
                "
                :style="{ backgroundImage: `url(${product.imageUrl})` }"
                alt=""
              ></div>
            </td>
            <td>
              {{ product.title }}
            </td>
            <td>
              <div class="h5" v-if="product.price === product.origin_price">
                {{ product.price }} 元
              </div>
              <div v-else>
                <del class="h6">原價 {{ product.origin_price }} 元</del>
                <div class="h5">現在只要 {{ product.price }} 元</div>
              </div>
            </td>
            <td>
              <div class="btn-group btn-group-sm">
                <button
                  type="button"
                  class="btn btn-outline-secondary"
                  @click="getProductDetail(product.id)"
                >
                  <i class="fas fa-spinner fa-pulse"></i>
                  查看更多
                </button>
                <button
                  type="button"
                  class="btn btn-outline-danger"
                  @click="addToCart(product.id)"
                >
                  <i class="fas fa-spinner fa-pulse"></i>
                  加到購物車
                </button>
              </div>
            </td>
          </tr>
        </tbody>
      </table>
      <!-- 購物車列表 -->
      <div class="text-end">
        <button class="btn btn-outline-danger" type="button">清空購物車</button>
      </div>
      <table class="table align-middle">
        <thead>
          <tr>
            <th></th>
            <th>品名</th>
            <th style="width: 150px">數量/單位</th>
            <th>單價</th>
          </tr>
        </thead>
        <tbody>
          <!-- <template v-if="cart.carts"> -->
          <tr>
            <td>
              <button type="button" class="btn btn-outline-danger btn-sm">
                <i class="fas fa-spinner fa-pulse"></i>
                x
              </button>
            </td>
            <td>
              {{}}
              <div class="text-success">已套用優惠券</div>
            </td>
            <td>
              <div class="input-group input-group-sm">
                <div class="input-group mb-3">
                  <input min="1" type="number" class="form-control" />
                  <span class="input-group-text" id="basic-addon2">{{}}</span>
                </div>
              </div>
            </td>
            <td class="text-end">
              <small class="text-success">折扣價：</small>
              {{}}
            </td>
          </tr>
          <!-- </template> -->
        </tbody>
        <tfoot>
          <tr>
            <td colspan="3" class="text-end">總計</td>
            <td class="text-end">{{}}</td>
          </tr>
          <tr>
            <td colspan="3" class="text-end text-success">折扣價</td>
            <td class="text-end text-success">{{}}</td>
          </tr>
        </tfoot>
      </table>
    </div>
    <!-- <div class="my-5 row justify-content-center">
      <v-form ref="form" class="col-md-6" v-slot="{ errors }">
        <div class="mb-3">
          <label for="email" class="form-label">Email</label>
          <v-field
            id="email"
            name="email"
            type="email"
            class="form-control"
            :class="{ 'is-invalid': errors['email'] }"
            placeholder="請輸入 Email"
          ></v-field>
          <error-message name="email" class="invalid-feedback"></error-message>
        </div>

        <div class="mb-3">
          <label for="name" class="form-label">收件人姓名</label>
          <v-field
            id="name"
            name="姓名"
            type="text"
            class="form-control"
            :class="{ 'is-invalid': errors['姓名'] }"
            placeholder="請輸入姓名"
            rules="required"
          ></v-field>
          <error-message name="姓名" class="invalid-feedback"></error-message>
        </div>

        <div class="mb-3">
          <label for="tel" class="form-label">收件人電話</label>
          <v-field
            id="tel"
            name="電話"
            type="text"
            class="form-control"
            :class="{ 'is-invalid': errors['電話'] }"
            placeholder="請輸入電話"
          ></v-field>
          <error-message name="電話" class="invalid-feedback"></error-message>
        </div>

        <div class="mb-3">
          <label for="address" class="form-label">收件人地址</label>
          <v-field
            id="address"
            name="地址"
            type="text"
            class="form-control"
            :class="{ 'is-invalid': errors['地址'] }"
            placeholder="請輸入地址"
            rules="required"
          ></v-field>
          <error-message name="地址" class="invalid-feedback"></error-message>
        </div>

        <div class="mb-3">
          <label for="message" class="form-label">留言</label>
          <textarea
            id="message"
            class="form-control"
            cols="30"
            rows="10"
          ></textarea>
        </div>
        <div class="text-end">
          <button type="submit" class="btn btn-danger">送出訂單</button>
        </div>
      </v-form>
    </div> -->
  </div>
</template>
