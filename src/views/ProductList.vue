<template>
  <q-page padding>
    <q-toolbar>
      <q-toolbar-title>產品列表</q-toolbar-title>
      <q-btn label="新增產品" color="primary" @click="addProduct" />
    </q-toolbar>

    <div class="row q-col-gutter-md q-mt-md">
      <div
        class="col-xs-12 col-sm-6 col-md-4"
        v-for="product in products"
        :key="product.id"
      >
        <q-card bordered>
          <q-card-section>
            <div class="text-h6">{{ product.name }}</div>
            <div class="text-subtitle2 text-positive">
              價格: NTD {{ product.price.toFixed(2) }}
            </div>
          </q-card-section>

          <q-card-section>
            <q-chip
              color="blue"
              text-color="white"
              outline
              v-if="product.category"
            >
              {{ product.category }}
            </q-chip>
            <q-chip
              :color="product.inStock ? 'green' : 'red'"
              text-color="white"
              outline
            >
              {{ product.inStock ? "有庫存" : "缺貨" }}
            </q-chip>
          </q-card-section>

          <q-card-actions align="right">
            <q-btn
              label="編輯"
              color="primary"
              flat
              @click="editProduct(product.id)"
            />
            <q-btn
              label="刪除"
              color="negative"
              flat
              @click="deleteProduct(product.id)"
            />
          </q-card-actions>
        </q-card>
      </div>
    </div>
  </q-page>
</template>

<script setup lang="ts">
import { ref } from "vue";
import apiservice from "../utils/ApiService";

interface ProductDetail {
  id: number;
  name: string;
  price: number;
  category: string;
  inStock: boolean;
}

interface ProductList {
  productList: ProductDetail[];
}

const products = ref([] as ProductDetail[]);

// 新增產品
const addProduct = () => {
  alert("新增產品功能尚未實作！");
};

// 編輯產品
const editProduct = (id: number) => {
  alert(`正在編輯產品 ID: ${id}`);
};

// 刪除產品
const deleteProduct = (id: number) => {
  alert(`產品 ID: ${id} 已刪除`);
};

const fetchData = async () => {
  const data = await apiservice.fetchApiWithoutPromise<ProductList>(
    "/api/product.json"
  );
  if (data?.productList) {
    products.value = data.productList;
  }
};

fetchData();
</script>

<style scoped>
.row {
  display: flex;
  flex-wrap: wrap;
  justify-content: space-between;
}
.col-xs-12 {
  width: 100%;
}
.col-sm-6 {
  width: 48%;
}
.col-md-4 {
  width: 30%;
}
</style>
