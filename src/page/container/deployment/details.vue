<template>
  <el-card class="glabal-detail-card">
    <div class="font-container">deployment</div>

    <el-tabs
      v-model="data.activeName"
      class="deployment-tab"
      @tab-click="handleClick"
      @tab-change="handleChange"
    >
      <el-tab-pane label="详情" name="first">User</el-tab-pane>
      <el-tab-pane label="Pod" name="second">Config</el-tab-pane>
      <el-tab-pane label="事件" name="third">Role</el-tab-pane>
      <el-tab-pane label="监控" name="fourth">Task</el-tab-pane>
    </el-tabs>
  </el-card>
</template>

<script setup lang="jsx">
import { useRouter } from 'vue-router';
import { reactive, getCurrentInstance, onMounted } from 'vue';
import { ElMessage, ElMessageBox } from 'element-plus';
import PixiuTag from '@/components/pixiuTag/index.vue';

const { proxy } = getCurrentInstance();
const router = useRouter();

const data = reactive({
  cluster: '',
  name: '',
  namespace: '',

  deployObject: '',

  loading: false,
  activeName: 'first',
});

onMounted(() => {
  data.cluster = proxy.$route.query.cluster;
  data.name = proxy.$route.query.name;
  data.namespace = proxy.$route.query.namespace;

  getDeployment();
});

const getDeployment = async () => {
  const res = await proxy.$http({
    method: 'get',
    url: `/proxy/pixiu/${data.cluster}/apis/apps/v1/namespaces/${data.namespace}/deployments/${data.name}`,
  });
  data.deployment = res;
};

const handleClick = (tab, event) => {
  console.log(data.activeName);
};

const handleChange = (name) => {
  console.log(data.activeName);
};
</script>

<style scoped="scoped">
.deployment-tab {
  margin-top: 20px;
}

.demo-tabs .el-tabs__content {
  padding: 32px;
  color: #6b778c;
  font-size: 32px;
  font-weight: 600;
}
</style>
