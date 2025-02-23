<script setup lang="ts">
import { onMounted, ref } from 'vue';
import gsap from 'gsap';
import { ScrollTrigger } from 'gsap/ScrollTrigger';

gsap.registerPlugin(ScrollTrigger);

onMounted(() => {
  // Анимация заголовка
  gsap.from('.hero-title', {
    duration: 2,
    y: -100,
    opacity: 0,
    ease: 'power4.out'
  });

  // Анимация военной техники
  gsap.from('.military-equipment', {
    duration: 1.5,
    x: -200,
    opacity: 0,
    stagger: 0.3,
    scrollTrigger: {
      trigger: '.military-section',
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
    onComplete: animateStats // Запуск анимации чисел после появления блока
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
});

// Функция для анимации чисел
const animateStats = () => {
  const statValues = document.querySelectorAll('.stat-value');
  statValues.forEach((statValue) => {
    const targetNumber = parseInt(statValue.textContent!.replace(/\D/g, ''), 10); // Извлекаем число из текста
    gsap.fromTo(statValue, {
      textContent: 0
    }, {
      textContent: targetNumber,
      duration: 2,
      ease: 'power1.out',
      snap: { textContent: 1 }, // Округляет до целых чисел
      onUpdate: function() {
        statValue.textContent = Math.floor(this.targets()[0].textContent) + (statValue.textContent!.includes('%') ? '%' : ''); // Добавляем символы, если они есть
      }
    });
  });
};

const stats = ref([
  { value: '1000000', label: 'Токенов в обращении' },
  { value: '5000', label: 'Держателей' },
  { value: '100%', label: 'Безопасность' }
]);

const equipment = ref([
  { name: 'Танк Т-90', power: '1000 HP', img: 'https://cdnstatic.rg.ru/uploads/images/2022/06/26/rian_t-90_7e7.jpg' },
  { name: 'Истребитель Су-57', power: '1500 HP', img: 'https://cdn.tvc.ru/pictures/o/405/474.jpg' },
  { name: 'Подводная лодка', power: '2000 HP', img: 'https://naked-science.ru/wp-content/uploads/2018/10/field_image_uss-hawaii-ssn776.jpg' },
]);

const roadmapItems = ref([
  { phase: 'Фаза 1', title: 'Запуск токена', description: 'Первичное размещение и листинг на BLUM' },
  { phase: 'Фаза 2', title: 'Развитие экосистемы', description: 'Листинг на Stone fi' },
  { phase: 'Фаза 3', title: 'Глобальное расширение', description: 'Лок монет на 2 недели' },
  { phase: 'Фаза 4', title: 'Инновации', description: 'Листинг на DEX' }
]);
</script>

<template>
  <div class="app-container">
    <!-- Героический раздел -->
    <section class="hero-section">
      <div class="military-overlay"></div>
      <h1 class="hero-title">HERO</h1>
      <p class="hero-subtitle">Токен силы и мощи российской армии</p>
      <div class="cta-buttons">
        <a href="https://t.me/+8UdwoJLab1Q4MjAy"><button class="primary-btn">Купить токен</button></a>
        <a href="https://t.me/+EB1tmmJ_nco3YjU6"><button class="secondary-btn">Telegram</button></a>
      </div>
    </section>

    <!-- Секция статистики -->
    <section class="statistics">
      <div class="stat-container">
        <div v-for="(stat, index) in stats" :key="index" class="stat-item">
          <div class="stat-value">{{ stat.value }}</div>
          <div class="stat-label">{{ stat.label }}</div>
        </div>
      </div>
    </section>

<!-- Военная техника -->
<section class="military-section">
  <h2>Военная мощь</h2>
  <div class="equipment-container">
    <div v-for="(item, index) in equipment" :key="index" class="military-equipment">
      <div class="equipment-image">
        <img :src="item.img" :alt="item.name">
      </div>
      <h3>{{ item.name }}</h3>
      <p>Мощность: {{ item.power }}</p>
    </div>
  </div>
</section>

    <!-- Дорожная карта -->
    <section class="roadmap">
      <h2>Дорожная карта</h2>
      <div class="roadmap-container">
        <div v-for="(item, index) in roadmapItems" :key="index" class="roadmap-item">
          <div class="phase-marker">{{ item.phase }}</div>
          <div class="roadmap-content">
            <h3>{{ item.title }}</h3>
            <p>{{ item.description }}</p>
          </div>
        </div>
      </div>
    </section>

    <!-- Футер -->
    <footer class="footer">
  <div class="footer-content">
    <div class="footer-section">
      <h3>О проекте</h3>
      <p>ЗАЩИТНИК - первый военно-патриотический токен</p>
    </div>
    <div class="footer-section">
      <h3>Контакты</h3>
      <p>Email: info@defender-token.ru</p>
    </div>
  </div>
  <div class="footer-disclaimer">
    <p><strong>Disclaimer:</strong>The token is not a security or investment instrument and does not grant any rights to profits or governance. The use of the token is at your own risk, with no guarantees from the creator. Our team is not responsible for any losses 
      related to its use. The token may not be available in some 
      jurisdictions. Please consult a lawyer or financial advisor 
      before using.</p>
  </div>
</footer>

  </div>
</template>

<style lang="scss" scoped>
.app-container {
  background: #0a0c1b;
  color: #ffffff;
}

.hero-section {
  height: 100vh;
  display: flex;
  flex-direction: column;
  justify-content: center;
  align-items: center;
  position: relative;
  background: url('/233.gif') center/cover no-repeat;
  
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

.military-overlay {
  position: absolute;
  top: 0;
  left: 0;
  right: 0;
  bottom: 0;
  background: 
    linear-gradient(45deg, rgba(169, 3, 3, 0.1) 0%, rgba(0, 0, 0, 0) 70%),
    repeating-linear-gradient(45deg, rgba(0, 0, 0, 0.1) 0px, rgba(0, 0, 0, 0.1) 2px, transparent 2px, transparent 6px);
  pointer-events: none;
}

.hero-title {
  font-size: 6rem;
  font-weight: 900;
  text-transform: uppercase;
  margin-bottom: 1rem;
  position: relative;
  color: #ffffff;
  text-shadow: 
    0 0 10px rgba(255, 0, 0, 0.5),
    0 0 20px rgba(255, 0, 0, 0.3);
  z-index: 1;
}

.hero-subtitle {
  font-size: 1.5rem;
  margin-bottom: 2rem;
  position: relative;
  z-index: 1;
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
    transition: transform 0.3s ease;

    &:hover {
      transform: translateY(-3px);
    }
  }
}

.primary-btn {
  background: #ff3232;
  color: white;
}

.secondary-btn {
  background: transparent;
  border: 2px solid #ff3232 !important;
  color: white;
}

.statistics {
  padding: 4rem 2rem;
  background: #0f1225;
}

.stat-container {
  max-width: 1200px;
  margin: 0 auto;
  display: grid;
  grid-template-columns: repeat(auto-fit, minmax(250px, 1fr));
  gap: 2rem;
}

.stat-item {
  text-align: center;
  padding: 2rem;
  background: rgba(255, 255, 255, 0.05);
  border-radius: 10px;
  border: 1px solid rgba(255, 0, 0, 0.1);

  .stat-value {
    font-size: 2.5rem;
    font-weight: bold;
    color: #ff3232;
    margin-bottom: 0.5rem;
  }

  .stat-label {
    font-size: 1.2rem;
    color: #ffffff;
    opacity: 0.8;
  }
}

.military-section {
  padding: 6rem 2rem;
  background: #0a0c1b;
  text-align: center;

  h2 {
    font-size: 3rem;
    margin-bottom: 3rem;
    color: #ff3232;
  }
}

.equipment-container {
  display: flex;
  flex-wrap: wrap;
  justify-content: center; // Центрирование карточек по горизонтали
  gap: 2rem; // Отступ между карточками
  max-width: 1200px;
  margin: 0 auto;
}

.military-equipment {
  background: rgba(255, 255, 255, 0.05);
  border-radius: 15px;
  padding: 1.5rem;
  text-align: center;
  border: 1px solid rgba(255, 0, 0, 0.1);
  width: 100%; // Ширина по умолчанию
  max-width: 300px; // Ограничение максимальной ширины

  .equipment-image {
    margin-bottom: 1rem;
    display: flex;
    justify-content: center;
    align-items: center;
    height: 200px; // Фиксированная высота для контейнера картинки
    
    img {
      width: 100%;
      height: auto; // Сохранение пропорций
      border-radius: 10px;
      object-fit: cover; // Чтобы картинка не искажалась
    }
  }

  h3 {
    color: #ff3232;
    margin-bottom: 0.5rem;
  }
}

@media (max-width: 768px) {
  .military-section {
    padding: 3rem 1rem; // Уменьшение отступов на мобильных устройствах
  }

  .equipment-container {
    gap: 1rem; // Уменьшение отступов между карточками
  }

  .military-equipment {
    max-width: 100%; // На мобильных устройствах карточки занимают всю ширину
    padding: 1rem; // Уменьшение отступов внутри карточек

    .equipment-image {
      height: 150px; // Уменьшение высоты контейнера картинки
    }
  }
}

.roadmap {
  padding: 6rem 2rem;
  background: #0f1225;

  h2 {
    text-align: center;
    font-size: 3rem;
    margin-bottom: 3rem;
    color: #ff3232;
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
    background: #ff3232;
    transform: translateX(-50%);
  }
}

.roadmap-item {
  display: flex;
  justify-content: space-between;
  align-items: center;
  margin-bottom: 3rem;
  position: relative;

  &:nth-child(even) {
    flex-direction: row-reverse;
  }

  .phase-marker {
    width: 120px;
    height: 40px;
    background: #ff3232;
    display: flex;
    align-items: center;
    justify-content: center;
    border-radius: 20px;
    font-weight: bold;
  }

  .roadmap-content {
    width: calc(50% - 80px);
    padding: 2rem;
    background: rgba(255, 255, 255, 0.05);
    border-radius: 10px;
    border: 1px solid rgba(255, 0, 0, 0.1);

    h3 {
      color: #ff3232;
      margin-bottom: 1rem;
    }
  }
}

.footer {
  background: #080a15;
  padding: 4rem 2rem;
  
  .footer-content {
    max-width: 1200px;
    margin: 0 auto;
    display: grid;
    grid-template-columns: repeat(auto-fit, minmax(250px, 1fr));
    gap: 2rem;
  }

  .footer-section {
    h3 {
      color: #ff3232;
      margin-bottom: 1rem;
    }

    p {
      color: #ffffff;
      opacity: 0.8;
    }
  }
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
</style>