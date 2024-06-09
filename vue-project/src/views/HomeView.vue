<template>
  <el-container class="layout-container">
    <NavBar />
    <el-container class="main-container">
      <el-header v-if="loginFlag" class="header">
        <strong>欢迎来到Joker数据标注平台</strong>
        <el-dropdown @command="handleCommand" placement="bottom-end" class="dropdown">
          <span class="el-dropdown__box">
            <el-avatar :src="avatar" />
            <el-icon><CaretBottom /></el-icon>
          </span>
          <template #dropdown>
            <el-dropdown-menu>
              <el-dropdown-item command="logout" :icon="SwitchButton">退出登录</el-dropdown-item>
            </el-dropdown-menu>
          </template>
        </el-dropdown>
      </el-header>
      <el-header v-else class="header">
        <div>
          <strong>欢迎来到Joker数据标注平台！</strong>
          <el-button type="primary" @click="goToLogin" style="margin-left: 20px;">登录</el-button>
        </div>
      </el-header>
      <el-row>
        <el-col :span="24">
          <!--数据标注任务-->
          <el-main class="main-style">
            <div class="basic-types-area">
              <el-radio-group v-model="selectedItem" @change="handleClickItem" class="radio-area">
                <el-radio-button label="0">单选题标注</el-radio-button>
                <el-radio-button label="1">多选题标注</el-radio-button>
                <el-radio-button label="2">填空题标注</el-radio-button>
              </el-radio-group>
              <div class="basic-types-content">
                <el-carousel class="carousel-box" ref="carousel" @change="carouselChange" :initial-index="0">
                  <el-carousel-item v-for="(item, index) in carouselItems" :key="index">
                    <el-row>
                      <el-col :span="12">
                        <el-image class="image" :src="item.image" fit="cover" />
                      </el-col>
                      <el-col :span="12">
                        <div class="custom-box1">
                          <div class="carousel-item-title">{{ item.title }}</div>
                          <div class="tag-area">
                            <el-tag v-for="tag in item.tags" :key="tag" style="margin-right: 10px">{{ tag }}</el-tag>
                          </div>
                          <div class="carousel-item-content">{{ item.description }}</div>
                        </div>
                      </el-col>
                    </el-row>
                  </el-carousel-item>
                </el-carousel>
              </div>
            </div>
          </el-main>
        </el-col>
        <!--数据标注平台-->
        <el-col :span="15">
          <el-main class="platform-introduction">
            <el-card class="info-card" shadow="hover">
              <div class="card-header">
                <h2 class="section-title">我们的数据标注平台</h2>
              </div>
              <el-row gutter="20">
                <el-col :span="12">
                  <div class="content-container">
                    <h3>全球部署的标注工具</h3>
                    <p>支持多种格式，支持与客户系统对接。</p>
                    <ul>
                      <li>提供全面的标注工具</li>
                      <li>灵活的项目管理流程</li>
                      <li>层级式的组与人员管理方式</li>
                    </ul>
                    <el-button type="primary">了解更多</el-button>
                  </div>
                </el-col>
                <el-col :span="12">
                  <el-image class="platform-image" :src="require('@/assets/home_img1.png')" />
                </el-col>
              </el-row>
            </el-card>
          </el-main>
        </el-col>
      </el-row>
    </el-container>
  </el-container>
</template>


<script setup>
import { ref, onMounted } from 'vue';
import { useRouter } from 'vue-router';
import { SwitchButton, CaretBottom } from '@element-plus/icons-vue';
import avatar from '@/assets/default.png';
import NavBar from '@/components/NavBar.vue';

// 数据定义
const carouselItems = ref([
  { id: 1, title: '单选题标注', image: require('@/assets/single_choice.jpg'), description: '用户根据图片内容选择一个正确的答案。', tags: ['图片', '单选题'] },
  { id: 2, title: '多选题标注', image: require('@/assets/multiple_choice.jpg'), description: '用户可以选择多个答案作为图片内容的描述。', tags: ['图片', '多选题'] },
  { id: 3, title: '填空题标注', image: require('@/assets/fill_in_blanks.jpg'), description: '用户需要根据图片内容填写适当的文字。', tags: ['图片', '填空题'] }
]);

const username = ref('');
const router = useRouter();
const loginFlag = ref(false);
const selectedItem = ref(0);
const carousel = ref(null);

onMounted(() => {
  username.value = sessionStorage.getItem('username') || '未登录用户';
  loginFlag.value = sessionStorage.getItem('logined') === 'true';
});

const handleCommand = (command) => {
  if (command === 'logout') {
    sessionStorage.removeItem('logined');
    sessionStorage.removeItem('username');
    router.push('/login');
  }
};

const goToLogin = () => {
  router.push('/login');
};

const handleClickItem = (label) => {
  selectedItem.value = Number(label);
  carousel.value.setActiveItem(selectedItem.value);
};

const carouselChange = (cur) => {
  selectedItem.value = cur;
};
</script>

<style scoped>
.main-container {
  display: flex;
  height: 100vh;
}

.section-title {
  font-size: 20px;
}


.radio-area {
  margin-bottom: 20px;
}

.el-carousel__button {
  border: none;
}

.platform-introduction .info-card {
  background-color: rgba(255, 255, 255, 0.7);
  border: none;
}

.platform-image {
  width: 100%;
  height: auto;
  object-fit: cover;
  background-image: url('@/assets/home_img1.png');
}

.section-title {
  font-size: 20px;
  text-align: left;
  margin-bottom: 20px;
}

.data-intro-card h2 {
  margin-bottom: 12px;
}



.layout-container {
  height: 100vh;
  background-image: url('@/assets/bkground.jpg');
  background-size: cover;
  background-position: center;
}

.header {
  font-size: large;
  display: flex;
  justify-content: center;
  align-items: center;
}

.dropdown {
  margin-left: auto;
}

.el-radio-button {
  margin: 0 10px;
}

.basic-types-content {
  width: 60%;
  display: flex;
  flex-direction: column;
  align-items: center;
}

.carousel-box {
  width: 100%;
}

.custom-box1 {
  display: flex;
  flex-direction: column;
  justify-content: center;
  align-items: flex-start;
  padding: 20px;
  border: none;
}

.carousel-item-title {
  font-size: 20px;
  font-weight: bold;
  margin-bottom: 10px;
  color: #333;
}

.tag-area {
  margin-bottom: 20px;
}

.carousel-item-content {
  font-size: 16px;
  color: #666;
}

.header strong {
  font-size: 20px;
}

ul {
  list-style: none;
  padding-left: 0;
}

ul li {
  position: relative;
  padding-left: 20px;
}

ul li::before {
  content: '';
  position: absolute;
  left: 0;
  top: 50%;
  transform: translateY(-50%);
  height: 6px;
  width: 6px;
  background-color: blue;
  border-radius: 50%;
}

</style>
