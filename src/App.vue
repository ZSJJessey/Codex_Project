<script setup>
import { computed, reactive, ref } from "vue";

const today = new Date();
const todayValue = `${today.getFullYear()}-${String(today.getMonth() + 1).padStart(2, "0")}-${String(
  today.getDate(),
).padStart(2, "0")}`;

const navItems = [
  { label: "菜单", href: "#menu" },
  { label: "风味", href: "#story" },
  { label: "到店", href: "#visit" },
];

const stats = [
  { value: "18:00", label: "每日晚餐开席" },
  { value: "6", label: "款季节主菜" },
  { value: "32", label: "个舒适座位" },
];

const dishes = [
  {
    name: "炭烤桂花鸡",
    description: "炭火烤出脆香鸡皮，刷桂花蜜汁，配山椒青菜解腻",
    price: "¥88",
    imageClass: "dish-one",
  },
  {
    name: "菌菇黄油饭",
    description: "时令菌菇与发酵黄油拌入热饭，最后点香葱油提香",
    price: "¥66",
    imageClass: "dish-two",
  },
  {
    name: "陈皮梅子排骨",
    description: "排骨慢煮至软糯，陈皮梅子酱酸甜收口，烤蒜泥增香",
    price: "¥92",
    imageClass: "dish-three",
  },
  {
    name: "松茸老火汤",
    description: "松茸、土鸡与竹荪小火慢炖，汤色清亮，入口回甘",
    price: "¥58",
    imageClass: "dish-four",
  },
];

const visitItems = [
  { title: "地址", text: "杭州市钱塘江" },
  { title: "营业", text: "周二至周日 18:00 - 23:00" },
  { title: "电话", text: "021-8800-2717" },
];

const times = ["18:00", "18:30", "19:00", "19:30", "20:00", "20:30"];

const reservation = reactive({
  name: "",
  date: todayValue,
  time: "",
  guests: 2,
});

const status = ref("");
const minDate = computed(() => todayValue);

function submitReservation() {
  const name = reservation.name.trim() || "客人";
  status.value = `${name}，已收到 ${reservation.date} ${reservation.time} 的 ${reservation.guests} 人预订请求。`;

  reservation.name = "";
  reservation.date = todayValue;
  reservation.time = "";
  reservation.guests = 2;
}
</script>

<template>
  <header class="site-header">
    <a class="brand" href="#home" aria-label="栖味小馆首页">
      <span class="brand-mark">Q</span>
      <span>栖味小馆</span>
    </a>
    <nav class="main-nav" aria-label="主导航">
      <a v-for="item in navItems" :key="item.href" :href="item.href">{{ item.label }}</a>
    </nav>
    <a class="nav-action" href="#reserve">预订</a>
  </header>

  <main id="home">
    <section class="hero" aria-label="栖味小馆">
      <div class="hero-content">
        <p class="eyebrow">晚餐 · 小酌 · 城市聚会</p>
        <h1>栖味小馆</h1>
        <p class="hero-copy">
          用当季食材做一桌有烟火气的菜。今晚留一张靠窗的桌，慢慢吃，慢慢聊。
        </p>
        <div class="hero-actions">
          <a class="primary-button" href="#reserve">预订座位</a>
          <a class="secondary-button" href="#menu">查看菜单</a>
        </div>
      </div>
    </section>

    <section class="intro-band" aria-label="餐厅特色">
      <div v-for="stat in stats" :key="stat.label">
        <span class="metric">{{ stat.value }}</span>
        <span>{{ stat.label }}</span>
      </div>
    </section>

    <section class="section menu-section" id="menu">
      <div class="section-heading">
        <p class="eyebrow">今日推荐</p>
        <h2>菜单精选</h2>
      </div>
      <div class="menu-grid">
        <article v-for="dish in dishes" :key="dish.name" class="menu-card">
          <div class="dish-photo" :class="dish.imageClass"></div>
          <div class="dish-info">
            <h3>{{ dish.name }}</h3>
            <p>{{ dish.description }}</p>
            <span>{{ dish.price }}</span>
          </div>
        </article>
      </div>
    </section>

    <section class="section story-section" id="story">
      <div class="story-copy">
        <p class="eyebrow">我们的风味</p>
        <h2>熟悉的家常底味，做得更细一点。</h2>
        <p>
          厨房每天按市场新鲜度调整小菜和主菜，保留中式餐桌的热闹，也把节奏放慢。
          从第一碟凉菜到最后一碗甜汤，每一道都适合分享。
        </p>
      </div>
      <div class="story-panel">
        <span>招牌组合</span>
        <strong>两人晚餐</strong>
        <p>前菜两道、主菜两道、米饭或面点、当日甜品。</p>
      </div>
    </section>

    <section class="section visit-section" id="visit">
      <div>
        <p class="eyebrow">到店信息</p>
        <h2>地址与营业时间</h2>
      </div>
      <div class="visit-grid">
        <div v-for="item in visitItems" :key="item.title">
          <h3>{{ item.title }}</h3>
          <p>{{ item.text }}</p>
        </div>
      </div>
    </section>

    <section class="reserve-section" id="reserve">
      <div class="reserve-copy">
        <p class="eyebrow">今晚有空位</p>
        <h2>预订一张桌</h2>
        <p>留下到店时间和人数，我们会尽快确认座位。</p>
      </div>
      <form class="reserve-form" @submit.prevent="submitReservation">
        <label>
          姓名
          <input v-model="reservation.name" type="text" name="name" placeholder="你的称呼" required />
        </label>
        <label>
          日期
          <input v-model="reservation.date" type="date" name="date" :min="minDate" required />
        </label>
        <label>
          时间
          <select v-model="reservation.time" name="time" required>
            <option value="">选择时间</option>
            <option v-for="time in times" :key="time">{{ time }}</option>
          </select>
        </label>
        <label>
          人数
          <input v-model.number="reservation.guests" type="number" name="guests" min="1" max="8" required />
        </label>
        <button type="submit">提交预订</button>
        <p class="form-status" role="status" aria-live="polite">{{ status }}</p>
      </form>
    </section>
  </main>

  <footer class="site-footer">
    <span>栖味小馆</span>
    <span>季节菜单每两周更新</span>
  </footer>
</template>
