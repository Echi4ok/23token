<script setup lang="ts">
import { onMounted, ref } from 'vue';
import gsap from 'gsap';
import { ScrollTrigger } from 'gsap/ScrollTrigger';

gsap.registerPlugin(ScrollTrigger);



onMounted(() => {
  // Создаем ракеты

  
  // Анимация заголовка
  gsap.from('.hero-title', {
    duration: 2,
    y: -100,
    opacity: 0,
    ease: 'power4.out'
  });

  // Анимация графиков
  gsap.from('.trading-item', {
    duration: 1.5,
    x: -200,
    opacity: 0,
    stagger: 0.3,
    scrollTrigger: {
      trigger: '.trading-section',
      start: 'top center',
      end: 'bottom center',
      toggleActions: 'play none none reverse'
    }
  });

  // Анимация статистики
  gsap.from('.stat-item', {
    duration: 1,
    scale: 0,
    opacity: 0,
    stagger: 0.2,
    scrollTrigger: {
      trigger: '.statistics',
      start: 'top center'
    },
    onComplete: animateStats
  });

  // Анимация дорожной карты
  gsap.from('.roadmap-item', {
    duration: 1.5,
    x: index => index % 2 === 0 ? -100 : 100,
    opacity: 0,
    stagger: 0.3,
    scrollTrigger: {
      trigger: '.roadmap',
      start: 'top center'
    }
  });

  // Интерактивные элементы
  document.querySelectorAll('.interactive-element').forEach(el => {
    el.addEventListener('mouseenter', () => {
      gsap.to(el, { scale: 1.05, duration: 0.3 });
    });
    el.addEventListener('mouseleave', () => {
      gsap.to(el, { scale: 1, duration: 0.3 });
    });
  });
});

// Функция для анимации чисел
const animateStats = () => {
  const statValues = document.querySelectorAll('.stat-value');
  statValues.forEach((statValue) => {
    const targetNumber = parseInt(statValue.textContent!.replace(/\D/g, ''), 10);
    gsap.fromTo(statValue, {
      textContent: 0
    }, {
      textContent: targetNumber,
      duration: 2,
      ease: 'power1.out',
      snap: { textContent: 1 },
      onUpdate: function() {
        statValue.textContent = Math.floor(this.targets()[0].textContent) + (statValue.textContent!.includes('%') ? '%' : '');
      }
    });
  });
};





const stats = ref([
  { value: '1000000', label: 'Токенов в обращении' },
  { value: '5000', label: 'Держателей' },
  { value: '100%', label: 'Волатильность' }
]);

const tradingItems = ref([
  { name: 'График BTC/USDT', description: 'Анализ ключевых уровней', img: '/btc.PNG' },
  { name: 'токен от Snoopy', description: 'Эксклюзивные торговые идеи', img: '/s.PNG' },
  { name: 'График TON/USDT', description: 'Точные входы по рынку', img: '/TION.PNG' },
]);

const roadmapItems = ref([
  { phase: '🌟 Фаза 1', title: '🚀 Запуск токена', description: 'Грандиозное первичное размещение и листинг на крупнейших DEX!' },
  { phase: '🤝 Фаза 2', title: '🔥 Развитие комьюнити', description: 'Запуск уникального приватного клуба трейдеров, где каждый сможет стать экспертом!' },
  { phase: '📈 Фаза 3', title: '🔍 Эксклюзивный контент', description: 'Доступ к инсайдерской аналитике от Snoopy для наших лучших инвесторов!' },
  { phase: '🎨 Фаза 4', title: '🛍️ Мерч и NFT', description: 'Производство уникальных коллекционных токенов и мерча для нашей преданной аудитории!' }
]);



</script>

<template>
  <div class="app-container" >

    <!-- Героический раздел -->
    <section class="hero-section">
      <div class="trading-overlay">
    <img src="/r.jpg" class="hero-image"/>
  </div>
  <h1 class="hero-title interactive-element">SNOOPY COIN</h1>
  <p class="hero-subtitle interactive-element">Токен легендарного трейдера</p>
  <div class="cta-buttons">
    <a href="https://t.me/snoopy_coin_official"><button class="primary-btn interactive-element">Купить токен</button></a>
    <a href="https://t.me/snoopy_trading_signals"><button class="secondary-btn interactive-element">Торговые сигналы</button></a>
  </div>
      
      <!-- Анимированный график на фоне -->
      <div class="animated-chart">
        <div class="chart-line" v-for="i in 5" :key="i" :style="{ height: `${10 + Math.random() * 80}%` }"></div>
      </div>
    </section>

    <!-- Секция статистики -->
    <section class="statistics">
      <div class="stat-container">
        
        <div v-for="(stat, index) in stats" :key="index" class="stat-item interactive-element">
          <p>Наша цель</p>
          <div class="stat-value">{{ stat.value }}</div>
          <div class="stat-label">{{ stat.label }}</div>
          <div class="stat-rocket">🚀</div>
        </div>
      </div>
    </section>

    <!-- Торговая секция -->
    <section class="trading-section">
      <h2 class="interactive-element">Трейдинг с Snoopy</h2>
      <div class="trading-container">
        <div v-for="(item, index) in tradingItems" :key="index" class="trading-item interactive-element">
          <div class="trading-image">
            <img :src="item.img" :alt="item.name">
            <div class="price-ticker">
              <span class="price-up">▲ {{ (Math.random() * 1000).toFixed(2) }}</span>
            </div>
          </div>
          <h3>{{ item.name }}</h3>
          <p>{{ item.description }}</p>
          <a href="https://t.me/snupitrayder"><button class="chart-btn">Показать график 🚀</button></a>
        </div>
      </div>
    </section>

    <!-- Дорожная карта -->
    <section class="roadmap">
      <h2 class="interactive-element">Дорожная карта</h2>
      <div class="roadmap-container">
        <div v-for="(item, index) in roadmapItems" :key="index" class="roadmap-item interactive-element">
          <div class="phase-marker">{{ item.phase }}</div>
          <div class="roadmap-content">
            <h3>{{ item.title }}</h3>
            <p>{{ item.description }}</p>
            <div class="progress-rocket">🚀</div>
          </div>
        </div>
      </div>
    </section>

    <!-- Футер -->
    <footer class="footer">
      <div class="footer-content">
        <div class="footer-section interactive-element">
          <h3>О проекте</h3>
          <p>Snoopy Coin - токен легендарного трейдера с эксклюзивными возможностями</p>
          <div class="footer-rocket">🚀</div>
        </div>
        <div class="footer-section interactive-element">
          <h3>Социальные сети</h3>
          <p>Telegram: <a href="https://t.me/snupitrayder">Официальный канал</a></p>
          <p>Telegram: <a href="https://t.me/snupitrayder">Торговые сигналы</a></p>
          <a href="https://t.me/snupitrayder"><button class="rocket-btn">Запустить все ракеты 🚀🚀🚀</button></a>
        </div>
      </div>
    </footer>
  </div>
</template>

<style lang="scss" scoped>
.hero-section {
  height: 100vh;
  display: flex;
  flex-direction: column;
  justify-content: center;
  align-items: center;
  position: relative;
  overflow: hidden; // Обрезаем всё, что выходит за границы
  
  &::before {
    content: '';
    position: absolute;
    top: 0;
    left: 0;
    right: 0;
    bottom: 0;
    background: rgba(10, 12, 27, 0.7);
    z-index: 1;
  }
}

.trading-overlay {
  position: absolute;
  top: 0;
  left: 0;
  right: 0;
  bottom: 0;
  display: flex;
  justify-content: center;
  align-items: center;
  overflow: hidden;
}

.hero-image {
  min-width: 100%;
  min-height: 100%;
  object-fit: cover; // Обрезает изображение, сохраняя пропорции
  position: absolute;
  top: 50%;
  left: 50%;
  transform: translate(-50%, -50%); // Точное центрирование
  width: 100%;
  height: auto;
  opacity: 0.8;
}

.hero-title, 
.hero-subtitle, 
.cta-buttons {
  position: relative;
  z-index: 2; // Помещаем поверх изображения
}
.app-container {
  background: #0a0c1b;
  color: #ffffff;
  font-family: 'Arial', sans-serif;
  overflow: hidden;
  position: relative;
}

.flying-rocket {
  position: fixed;
  font-size: 20px;
  z-index: 100;
  pointer-events: none;
  opacity: 0.8;
  transform: rotate(45deg);
}

.rocket-launch {
  position: fixed;
  font-size: 24px;
  z-index: 1000;
  pointer-events: none;
}

.hero-section {
  height: 100vh;
  display: flex;
  flex-direction: column;
  justify-content: center;
  align-items: center;
  position: relative;
  background: url('https://wallpapercave.com/wp/wp2752748.jpg') center/cover no-repeat;
  
  &::before {
    content: '';
    position: absolute;
    top: 0;
    left: 0;
    right: 0;
    bottom: 0;
    background: rgba(10, 12, 27, 0.7);
  }
}

.animated-chart {
  position: absolute;
  bottom: 0;
  left: 0;
  width: 100%;
  height: 30%;
  display: flex;
  align-items: flex-end;
  justify-content: space-around;
  padding: 0 20px;
  z-index: 0;
  
  .chart-line {
    width: 3%;
    background: rgba(0, 200, 255, 0.3);
    margin: 0 1%;
    animation: chartAnimation 3s infinite alternate;
    
    @for $i from 1 through 5 {
      &:nth-child(#{$i}) {
        animation-delay: $i * 0.2s;
      }
    }
  }
}

@keyframes chartAnimation {
  0% { height: 10%; }
  100% { height: 90%; }
}

.trading-overlay {
  position: absolute;
  top: 0;
  left: 0;
  right: 0;
  bottom: 0;
  background: 
    linear-gradient(45deg, rgba(0, 200, 255, 0.1) 0%, rgba(0, 0, 0, 0) 70%),
    repeating-linear-gradient(45deg, rgba(0, 0, 0, 0.1) 0px, rgba(0, 0, 0, 0.1) 2px, transparent 2px, transparent 6px);
  pointer-events: none;
}

.hero-title {
  font-size: 6rem;
  font-weight: 900;
  text-transform: uppercase;
  margin-bottom: 1rem;
  position: relative;
  color: #00c8ff;
  text-shadow: 
    0 0 10px rgba(0, 200, 255, 0.5),
    0 0 20px rgba(0, 200, 255, 0.3);
  z-index: 1;
  transition: all 0.3s ease;
}

.hero-subtitle {
  font-size: 1.5rem;
  margin-bottom: 2rem;
  position: relative;
  z-index: 1;
  color: #ffffff;
  transition: all 0.3s ease;
}

.cta-buttons {
  display: flex;
  gap: 1rem;
  z-index: 1;

  button {
    padding: 1rem 2rem;
    font-size: 1.2rem;
    border: none;
    border-radius: 5px;
    cursor: pointer;
    transition: all 0.3s ease;
    position: relative;
    overflow: hidden;

    &:hover {
      transform: translateY(-3px);
      box-shadow: 0 5px 15px rgba(0, 200, 255, 0.4);
      
      &::after {
        content: '🚀';
        position: absolute;
        right: 10px;
        animation: rocketFly 0.5s forwards;
      }
    }
  }
}

@keyframes rocketFly {
  0% { transform: translateX(0) translateY(0); opacity: 1; }
  100% { transform: translateX(100px) translateY(-50px); opacity: 0; }
}

.primary-btn {
  background: #00c8ff;
  color: #0a0c1b;
  font-weight: bold;
}

.secondary-btn {
  background: transparent;
  border: 2px solid #00c8ff !important;
  color: #00c8ff;
  font-weight: bold;
}

.statistics {
  padding: 4rem 2rem;
  background: #0f1225;
  position: relative;
}

.stat-container {
  max-width: 1200px;
  margin: 0 auto;
  display: grid;
  grid-template-columns: repeat(auto-fit, minmax(250px, 1fr));
  gap: 2rem;
  position: relative;
}

.stat-item {
  text-align: center;
  padding: 2rem;
  background: rgba(255, 255, 255, 0.05);
  border-radius: 10px;
  border: 1px solid rgba(0, 200, 255, 0.1);
  transition: all 0.3s ease;
  position: relative;
  
  &:hover {
    transform: translateY(-10px);
    box-shadow: 0 10px 20px rgba(0, 200, 255, 0.2);
    
    .stat-rocket {
      animation: statRocket 1s forwards;
    }
  }

  .stat-value {
    font-size: 2.5rem;
    font-weight: bold;
    color: #00c8ff;
    margin-bottom: 0.5rem;
  }

  .stat-label {
    font-size: 1.2rem;
    color: #ffffff;
    opacity: 0.8;
  }
  
  .stat-rocket {
    position: absolute;
    top: -15px;
    right: -15px;
    font-size: 24px;
    opacity: 0;
  }
}

@keyframes statRocket {
  0% { transform: translateY(0) translateX(0); opacity: 1; }
  100% { transform: translateY(-50px) translateX(50px); opacity: 0; }
}

.trading-section {
  padding: 6rem 2rem;
  background: #0a0c1b;
  text-align: center;
  position: relative;

  h2 {
    font-size: 3rem;
    margin-bottom: 3rem;
    color: #00c8ff;
    transition: all 0.3s ease;
  }
}

.trading-container {
  display: flex;
  flex-wrap: wrap;
  justify-content: center;
  gap: 2rem;
  max-width: 1200px;
  margin: 0 auto;
}

.trading-item {
  background: rgba(255, 255, 255, 0.05);
  border-radius: 15px;
  padding: 1.5rem;
  text-align: center;
  border: 1px solid rgba(0, 200, 255, 0.1);
  width: 100%;
  max-width: 300px;
  transition: all 0.3s ease;
  position: relative;
  overflow: hidden;
  
  &:hover {
    transform: translateY(-10px);
    box-shadow: 0 10px 20px rgba(0, 200, 255, 0.2);
    
    .price-ticker {
      opacity: 1;
      transform: translateY(0);
    }
  }

  .trading-image {
    margin-bottom: 1rem;
    display: flex;
    justify-content: center;
    align-items: center;
    height: 200px;
    position: relative;
    overflow: hidden;
    
    img {
      width: 100%;
      height: auto;
      border-radius: 10px;
      object-fit: cover;
      transition: transform 0.5s ease;
    }
    
    &:hover img {
      transform: scale(1.1);
    }
    
    .price-ticker {
      position: absolute;
      bottom: 10px;
      left: 10px;
      background: rgba(0, 200, 255, 0.8);
      padding: 5px 10px;
      border-radius: 5px;
      opacity: 0;
      transform: translateY(20px);
      transition: all 0.3s ease;
      
      .price-up {
        color: #00ff00;
        font-weight: bold;
      }
    }
  }

  h3 {
    color: #00c8ff;
    margin-bottom: 0.5rem;
  }
  
  .chart-btn {
    margin-top: 15px;
    padding: 8px 15px;
    background: rgba(0, 200, 255, 0.2);
    border: 1px solid #00c8ff;
    color: white;
    border-radius: 5px;
    cursor: pointer;
    transition: all 0.3s ease;
    
    &:hover {
      background: #00c8ff;
      color: #0a0c1b;
    }
  }
}

.roadmap {
  padding: 6rem 2rem;
  background: #0f1225;
  position: relative;

  h2 {
    text-align: center;
    font-size: 3rem;
    margin-bottom: 3rem;
    color: #00c8ff;
    transition: all 0.3s ease;
  }
}

.roadmap-container {
  max-width: 1000px;
  margin: 0 auto;
  position: relative;

  &::before {
    content: '';
    position: absolute;
    top: 0;
    left: 50%;
    width: 2px;
    height: 100%;
    background: #00c8ff;
    transform: translateX(-50%);
  }
}

.roadmap-item {
  display: flex;
  justify-content: space-between;
  align-items: center;
  margin-bottom: 3rem;
  position: relative;
  transition: all 0.3s ease;

  &:hover {
    .progress-rocket {
      animation: progressRocket 1.5s infinite;
    }
  }

  &:nth-child(even) {
    flex-direction: row-reverse;
  }

  .phase-marker {
    width: 120px;
    height: 40px;
    background: #00c8ff;
    color: #0a0c1b;
    display: flex;
    align-items: center;
    justify-content: center;
    border-radius: 20px;
    font-weight: bold;
    position: relative;
    z-index: 2;
  }

  .roadmap-content {
    width: calc(50% - 80px);
    padding: 2rem;
    background: rgba(255, 255, 255, 0.05);
    border-radius: 10px;
    border: 1px solid rgba(0, 200, 255, 0.1);
    position: relative;
    
    &:hover {
      transform: translateY(-5px);
      box-shadow: 0 5px 15px rgba(0, 200, 255, 0.2);
    }

    h3 {
      color: #00c8ff;
      margin-bottom: 1rem;
    }
    
    .progress-rocket {
      position: absolute;
      right: 20px;
      top: 20px;
      font-size: 20px;
    }
  }
}

@keyframes progressRocket {
  0% { transform: translateX(0); }
  50% { transform: translateX(20px); }
  100% { transform: translateX(0); }
}

.footer {
  background: #080a15;
  padding: 4rem 2rem;
  position: relative;
  
  .footer-content {
    max-width: 1200px;
    margin: 0 auto;
    display: grid;
    grid-template-columns: repeat(auto-fit, minmax(250px, 1fr));
    gap: 2rem;
  }

  .footer-section {
    position: relative;
    padding: 20px;
    border-radius: 10px;
    transition: all 0.3s ease;
    
    &:hover {
      background: rgba(0, 200, 255, 0.1);
      
      .footer-rocket {
        animation: footerRocket 2s forwards;
      }
    }

    h3 {
      color: #00c8ff;
      margin-bottom: 1rem;
    }

    p, a {
      color: #ffffff;
      opacity: 0.8;
      text-decoration: none;
      transition: all 0.3s ease;
    }

    a:hover {
      color: #00c8ff;
    }
    
    .footer-rocket {
      position: absolute;
      bottom: 10px;
      right: 10px;
      font-size: 24px;
    }
    
    .rocket-btn {
      margin-top: 15px;
      padding: 10px 15px;
      background: rgba(0, 200, 255, 0.2);
      border: 1px solid #00c8ff;
      color: white;
      border-radius: 5px;
      cursor: pointer;
      transition: all 0.3s ease;
      
      &:hover {
        background: #00c8ff;
        color: #0a0c1b;
        box-shadow: 0 0 20px rgba(0, 200, 255, 0.5);
      }
    }
  }
}

@keyframes footerRocket {
  0% { transform: translateY(0) rotate(0); opacity: 1; }
  100% { transform: translateY(-100px) rotate(20deg); opacity: 0; }
}

@media (max-width: 768px) {
  .hero-title {
    font-size: 4rem;
  }

  .roadmap-container::before {
    left: 20px;
  }

  .roadmap-item {
    flex-direction: column !important;
    margin-left: 40px;

    .roadmap-content {
      width: 100%;
    }
  }
}

.interactive-element {
  cursor: pointer;
  transition: all 0.3s ease;
  
  &:hover {
    transform: scale(1.05);
  }
}
</style>