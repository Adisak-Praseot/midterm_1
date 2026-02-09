<template>
  <q-page padding>
    <div class="text-h4 q-mb-md">
      Advanced Full-Stack Demo (Quasar + Express)
    </div>

    <!-- Git Workflow Section -->
    <q-card class="q-mb-md">
      <q-card-section>
        <div class="text-h6">Git Workflow</div>
        <q-list bordered separator class="q-mt-sm">
          <q-item v-for="(step, index) in gitSteps" :key="index">
            <q-item-section avatar>
              <q-badge>{{ index + 1 }}</q-badge>
            </q-item-section>
            <q-item-section>
              <q-item-label>{{ step.title }}</q-item-label>
              <q-item-label caption>{{ step.detail }}</q-item-label>
            </q-item-section>
          </q-item>
        </q-list>
      </q-card-section>
    </q-card>

    <!-- Docker Concepts Section -->
    <q-card class="q-mb-md">
      <q-card-section>
        <div class="text-h6">Docker Concepts</div>
        <q-list bordered separator class="q-mt-sm">
          <q-item v-for="(item, index) in dockerItems" :key="index">
            <q-item-section>
              <q-item-label>{{ item.title }}</q-item-label>
              <q-item-label caption>{{ item.detail }}</q-item-label>
            </q-item-section>
          </q-item>
        </q-list>
      </q-card-section>
    </q-card>

    <!-- API Data from Backend -->
    <q-card>
      <q-card-section>
        <div class="text-h6">Data from Backend API</div>
        <div class="q-mb-md row items-center q-gutter-sm">
          <q-btn
            color="primary"
            label="Refresh Data"
            :loading="apiLoading"
            @click="fetchData"
          />
          <span v-if="apiError" class="text-negative">
            {{ apiError }}
          </span>
        </div>
        <q-spinner v-if="apiLoading" color="primary" size="2em" />
        <q-list v-else-if="apiData.git && apiData.docker" bordered separator class="q-mt-sm">
          <q-item>
            <q-item-section>
              <q-item-label>{{ apiData.git.title }}</q-item-label>
              <q-item-label caption>{{ apiData.git.detail }}</q-item-label>
            </q-item-section>
          </q-item>
          <q-item>
            <q-item-section>
              <q-item-label>{{ apiData.docker.title }}</q-item-label>
              <q-item-label caption>{{ apiData.docker.detail }}</q-item-label>
            </q-item-section>
          </q-item>
        </q-list>
      </q-card-section>
    </q-card>
  </q-page>
</template>

<script setup>
import { ref, onMounted } from 'vue';
import axios from 'axios';

// อ่านค่าจาก .env (Vite ต้องใช้ VITE_ prefix)
const API_URL = import.meta.env.VITE_API_URL || 'http://localhost:3000';

// Git Workflow Steps
const gitSteps = [
  {
    title: 'Create Feature Branch',
    detail: 'สร้าง branch ใหม่สำหรับฟีเจอร์ หรือ bugfix'
  },
  {
    title: 'Make Changes',
    detail: 'พัฒนา code และ commit เข้า branch'
  },
  {
    title: 'Create Pull Request',
    detail: 'ส่ง PR เพื่อให้ team review code'
  },
  {
    title: 'Code Review & Merge',
    detail: 'Reviewer ตรวจสอบ และ merge เข้า main branch'
  },
  {
    title: 'Advanced Strategy',
    detail: 'ใช้ branch protection, squash merge เพื่อ history สะอาด'
  }
];

// Docker Concepts
const dockerItems = [
  {
    title: 'Image',
    detail: 'Template แบบ read-only ที่เก็บ app, libraries, dependencies'
  },
  {
    title: 'Container',
    detail: 'Runtime instance ของ image ที่สามารถ run, start, stop ได้'
  },
  {
    title: 'Registry',
    detail: 'Repository ที่เก็บ images เช่น Docker Hub, Quay.io'
  },
  {
    title: 'Compose',
    detail: 'Tool สำหรับ define และ run multi-container Docker applications'
  },
  {
    title: 'Advanced',
    detail: 'Multi-stage build, healthcheck, volume, network management'
  }
];

// API Data
const apiData = ref({ git: {}, docker: {} });
const apiLoading = ref(true);
const apiError = ref('');

// Task data (ถ้ายังใช้)
const tasks = ref([]);
const loading = ref(false);
const errorMessage = ref('');

const fetchData = async () => {
  apiLoading.value = true;
  apiError.value = '';

  try {
    const response = await axios.get(API_URL + '/api/demo');
    apiData.value = response.data;
  } catch (error) {
    console.error('API Error:', error);
    apiError.value = 'ไม่สามารถโหลดข้อมูลจาก API ได้';
  } finally {
    apiLoading.value = false;
  }
};

const fetchTasks = async () => {
  loading.value = true;
  errorMessage.value = '';

  try {
    const res = await axios.get(API_URL + '/api/tasks');
    tasks.value = res.data.data;
  } catch (err) {
    console.error('API /api/tasks error:', err);
    errorMessage.value = 'โหลดงานจากฐานข้อมูลไม่สำเร็จ';
  } finally {
    loading.value = false;
  }
};

onMounted(() => {
  fetchData();
});
</script>
