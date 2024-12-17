<template>
  <q-page class="q-pa-md">
    <q-banner class="q-mb-md" color="primary" text-color="white">
      <div class="text-h6">歡迎來到我們的系統！</div>
    </q-banner>

    <div class="q-mt-md">
      <q-card>
        <q-card-section>
          <div class="text-h6">最新資訊</div>
          <q-separator />
          <q-list>
            <q-item v-for="news in newsList" :key="news.title" clickable>
              <q-item-section>
                <div class="text-body1">{{ news.title }}</div>
                <div class="text-caption">{{ news.date }}</div>
              </q-item-section>
            </q-item>
          </q-list>
        </q-card-section>
      </q-card>
    </div>

    <div class="q-mt-md">
      <q-card>
        <q-card-section v-if="eventDetails">
          <div class="text-h6">聖誕節活動</div>
          <q-separator />
          <div>
            <p>日期: {{ eventDetails.date }}</p>
            <p>描述: {{ eventDetails.description }}</p>
            <q-btn label="了解更多" color="primary" flat />
            <q-btn label="參加活動" color="secondary" flat />
          </div>
        </q-card-section>
      </q-card>
    </div>
  </q-page>
</template>

<script lang="ts">
import { defineComponent, ref } from "vue";
import axios from "axios";

interface User {
  id: number;
  name: string;
  email: string;
}

interface News {
  title: string;
  date: string;
}

interface Event {
  date: string;
  description: string;
}

export default defineComponent({
  setup() {
    const user = ref<User | null>(null);
    const newsList = ref<News[]>([]);
    const eventDetails = ref<Event>();

    const fetchNewsList = async () => {
      try {
        const response = await axios.get<News[]>("/api/news.json");
        newsList.value = response.data;
      } catch (error) {
        console.error("帶錯誤處理的 API 請求錯誤:", error);
      }
    };

    const fetchEventDetails = async () => {
      try {
        const response = await axios.get<Event>("/api/fetchEventDetails.json");
        eventDetails.value = response.data;
      } catch (error) {
        console.error("帶錯誤處理的 API 請求錯誤:", error);
      }
    };

    fetchNewsList();
    fetchEventDetails();

    return {
      user,
      newsList,
      eventDetails,
    };
  },
});
</script>

<style scoped>
.q-banner {
  margin-bottom: 16px;
}
.row {
  display: flex;
  flex-wrap: wrap;
}
.col-12 {
  width: 100%;
}
.col-md-6 {
  width: 48%;
  margin-right: 4%;
}
.q-card {
  min-height: 200px;
}
</style>
