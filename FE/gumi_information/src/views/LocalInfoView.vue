<!-- src/components/Calender.vue (또는 기존 위치) -->
<template>
  <div class="local-info-container">
    <!-- 상단 요약 & 월별/유형별 필터 탭 -->
    <div class="filter-header">
      <div class="tag-filters">
        <button 
          v-for="tab in tabs" 
          :key="tab.value"
          @click="activeTab = tab.value"
          :class="['filter-btn', { active: activeTab === tab.value }]"
        >
          {{ tab.label }}
        </button>
      </div>
      <span class="results-count">이번 달 예정 행사: 총 {{ filteredEvents.length }}개</span>
    </div>

    <!-- 지역 행사/축제 카드 리스트 -->
    <div class="places-grid">
      <div 
        v-for="event in filteredEvents" 
        :key="event.id" 
        class="place-card"
        @click="selectEvent(event)"
      >
        <div class="card-image-placeholder">
          <span class="emoji-icon">{{ event.emoji }}</span>
        </div>
        <div class="card-body">
          <span class="place-tag" :class="event.category">{{ getCategoryLabel(event.category) }}</span>
          <h3 class="place-title">{{ event.title }}</h3>
          <p class="place-date">📅 {{ event.date }}</p>
          <p class="place-addr">📍 {{ event.address }}</p>
          <p class="place-desc">{{ event.description }}</p>
          <div class="card-footer">
            <span class="like-badge">🔥 관심도 {{ event.likes }}</span>
            <span class="detail-btn-text">일정 상세 →</span>
          </div>
        </div>
      </div>
    </div>

    <!-- 행사 상세 보기 모달 창 -->
    <div v-if="selectedEvent" class="modal-overlay" @click.self="selectedEvent = null">
      <div class="modal-content">
        <button class="close-btn" @click="selectedEvent = null">✕</button>
        <div class="modal-hero">
          <span class="modal-emoji">{{ selectedEvent.emoji }}</span>
        </div>
        <div class="modal-body">
          <span class="place-tag" :class="selectedEvent.category">{{ getCategoryLabel(selectedEvent.category) }}</span>
          <h2>{{ selectedEvent.title }}</h2>
          <p class="modal-addr"><strong>일시:</strong> {{ selectedEvent.date }}</p>
          <p class="modal-addr"><strong>장소:</strong> {{ selectedEvent.address }}</p>
          <p class="modal-tel" v-if="selectedEvent.tel"><strong>문의처:</strong> {{ selectedEvent.tel }}</p>
          <hr class="divider" />
          <p class="modal-desc">{{ selectedEvent.details }}</p>
        </div>
      </div>
    </div>
  </div>
</template>

<script setup>
import { ref, computed } from 'vue'

// 카테고리 탭 목록 (행사 유형별 분류)
const activeTab = ref('all')
const tabs = [
  { label: '전체 일정', value: 'all' },
  { label: '🎉 축제/공연', value: 'festival' },
  { label: '🏛️ 전시/전통', value: 'culture' },
  { label: '🏃 체육/대회', value: 'sports' }
]

// 선택된 상세 보기 행사 (모달용)
const selectedEvent = ref(null)
const selectEvent = (event) => {
  selectedEvent.value = event
}

// 카테고리 한글 텍스트 반환용 헬퍼 함수
const getCategoryLabel = (cat) => {
  const labels = { festival: '축제/공연', culture: '전시/전통', sports: '체육/대회' }
  return labels[cat] || cat
}

// 구미/경북 지역 행사/축제 일정 캘린더 더미 데이터
const events = ref([
  {
    id: 1,
    title: '금오산 잔디광장 봄 락 페스티벌',
    category: 'festival',
    emoji: '🎸',
    date: '2026년 5월 15일 ~ 5월 17일',
    address: '경상북도 구미시 금오산로 402-4 잔디광장',
    tel: '054-480-4601',
    description: '푸른 금오산을 배경으로 펼쳐지는 구미 최대의 인디 밴드 및 락 음악 축제입니다.',
    details: '구미 시민과 경북 지역 주민들을 위해 매년 봄 개최되는 음악 축제입니다. 돗자리를 펴고 잔디밭에 앉아 무료로 공연을 관람할 수 있으며, 인근 금리단길 매장들과 연계한 다양한 푸드트럭 팝업스토어가 운영됩니다.',
    likes: 245
  },
  {
    id: 2,
    title: '낙동강 핑크뮬리 걷기 대회',
    category: 'sports',
    emoji: '🏃',
    date: '2026년 10월 10일 (토)',
    address: '경상북도 구미시 낙동강변로 820 체육공원 일원',
    tel: '054-480-6181',
    description: '가을바람과 함께 만개한 핑크뮬리 산책길을 따라 걷는 가족 중심의 건강 행사입니다.',
    details: '국내 최대 규모의 낙동강 체육공원 내 핑크뮬리 군락지를 따라 걷는 5km 코스 코스입니다. 완주자 전원에게는 지역 특산품 기념품을 증정하며, 페이스페인팅, 어린이 자전거 묘기 등 가족 단위 관람객을 위한 이벤트 부스가 다채롭게 마련됩니다.',
    likes: 189
  },
  {
    id: 3,
    title: '선산 선비문화재 및 오일장 풍물 전시',
    category: 'culture',
    emoji: '🌾',
    date: '2026년 9월 12일 ~ 9월 14일',
    address: '경상북도 구미시 선산읍 단계동길 24',
    tel: '054-480-2641',
    description: '유서 깊은 선산의 선비 정신과 영남 풍물놀이를 현대적으로 재해석한 전통 문화 행사입니다.',
    details: '조선시대 인재의 고장이라 불리던 선산의 역사적 가치를 기리는 문화제입니다. 장날(2일, 7일)과 연계하여 옛 선산 동헌 투어, 한복 입기 체험, 전통 떡메치기 및 장터 국밥 거리 시식 행사가 함께 진행됩니다.',
    likes: 132
  },
  {
    id: 4,
    title: '구미 과학관 우주 돔 텐트 별빛 캠프',
    category: 'culture',
    emoji: '🔭',
    date: '2026년 8월 22일 (토) 18:00',
    address: '경상북도 구미시 3공단1로 244-77 과학관 천체관측대',
    tel: '054-476-6501',
    description: '여름밤 하늘의 별자리와 은하수를 대형 망원경으로 직접 관측해보는 어린이 천체 캠프입니다.',
    details: '구미과학관 전문 연구원의 해설과 함께 여름철 대삼각형 별자리를 관측하는 프로그램입니다. 플라네타리움 스페셜 영상 관람 및 나만의 LED 별자리 무드등 만들기 워크숍이 포함되어 있어 사전 예약이 필수인 인기 일정입니다.',
    likes: 156
  },
  {
    id: 5,
    title: '경북도민 체육대회 구미 유치 기념 콘서트',
    category: 'festival',
    emoji: '🎤',
    date: '2026년 6월 05일 19:30',
    address: '경상북도 구미시 시민운동장 주경기장',
    tel: '',
    description: '경북도민체전 유치를 축하하기 위해 국내 유명 가수들이 총출동하는 특별 문화 축제입니다.',
    details: '도민체전의 성공적인 개최와 경북도민 화합을 위한 초대형 오픈 콘서트입니다. 화려한 드론 라이트쇼와 불꽃놀이가 피날레를 장식할 예정이며, 입장은 선착순 무료로 진행됩니다.',
    likes: 310
  }
])

// 탭 필터링 로직 (축제/문화/체육 등)
const filteredEvents = computed(() => {
  if (activeTab.value === 'all') return events.value
  return events.value.filter(event => event.category === activeTab.value)
})
</script>

<style scoped>
.local-info-container {
  display: flex;
  flex-direction: column;
  gap: 24px;
}

/* 필터 헤더 */
.filter-header {
  display: flex;
  justify-content: space-between;
  align-items: center;
  flex-wrap: wrap;
  gap: 12px;
  border-bottom: 1.5px solid #f1f3f5;
  padding-bottom: 16px;
}

.tag-filters {
  display: flex;
  gap: 8px;
}

.filter-btn {
  background-color: #f1f3f5;
  border: none;
  padding: 8px 16px;
  border-radius: 20px;
  font-size: 14px;
  font-weight: 600;
  color: #495057;
  cursor: pointer;
  transition: all 0.2s;
}

.filter-btn:hover {
  background-color: #e9ecef;
}

.filter-btn.active {
  background-color: #111;
  color: #fff;
}

.results-count {
  font-size: 14px;
  color: #868e96;
  font-weight: 500;
}

/* 축제 카드 그리드 */
.places-grid {
  display: grid;
  grid-template-columns: repeat(auto-fill, minmax(280px, 1fr));
  gap: 20px;
}

.place-card {
  background-color: #ffffff;
  border: 1px solid #e9ecef;
  border-radius: 12px;
  overflow: hidden;
  cursor: pointer;
  transition: transform 0.2s, box-shadow 0.2s;
}

.place-card:hover {
  transform: translateY(-5px);
  box-shadow: 0 8px 20px rgba(0,0,0,0.06);
}

.card-image-placeholder {
  background-color: #f8f9fa;
  height: 140px;
  display: flex;
  align-items: center;
  justify-content: center;
  border-bottom: 1px solid #e9ecef;
}

.emoji-icon {
  font-size: 48px;
}

.card-body {
  padding: 16px;
}

.place-tag {
  font-size: 11px;
  padding: 4px 8px;
  border-radius: 4px;
  font-weight: 700;
}
/* 카테고리별 다채로운 배지 컬러 분기 */
.place-tag.festival { color: #d9480f; background-color: #fff0f6; }
.place-tag.culture { color: #2b8a3e; background-color: #e8f5e9; }
.place-tag.sports { color: #1c7ed6; background-color: #e7f5ff; }

.place-title {
  font-size: 17px;
  font-weight: 800;
  margin: 10px 0 6px 0;
  color: #212529;
  line-height: 1.3;
}

.place-date {
  font-size: 13px;
  font-weight: 600;
  color: #e03131;
  margin: 0 0 4px 0;
}

.place-addr {
  font-size: 12px;
  color: #868e96;
  margin: 0 0 10px 0;
}

.place-desc {
  font-size: 13px;
  color: #495057;
  line-height: 1.4;
  margin: 0 0 16px 0;
  display: -webkit-box;
  -webkit-line-clamp: 2;
  -webkit-box-orient: vertical;
  overflow: hidden;
}

.card-footer {
  display: flex;
  justify-content: space-between;
  align-items: center;
  font-size: 13px;
  font-weight: 600;
}

.like-badge {
  color: #495057;
}

.detail-btn-text {
  color: #111;
}

/* 모달 스타일 */
.modal-overlay {
  position: fixed;
  top: 0;
  left: 0;
  right: 0;
  bottom: 0;
  background-color: rgba(0,0,0,0.5);
  display: flex;
  align-items: center;
  justify-content: center;
  z-index: 1000;
  padding: 16px;
}

.modal-content {
  background-color: #ffffff;
  border-radius: 16px;
  max-width: 500px;
  width: 100%;
  overflow: hidden;
  position: relative;
  box-shadow: 0 10px 30px rgba(0,0,0,0.15);
  animation: modalShow 0.25s ease-out;
}

@keyframes modalShow {
  from { transform: translateY(20px); opacity: 0; }
  to { transform: translateY(0); opacity: 1; }
}

.close-btn {
  position: absolute;
  top: 16px;
  right: 16px;
  background: rgba(0, 0, 0, 0.4);
  color: white;
  border: none;
  width: 32px;
  height: 32px;
  border-radius: 50%;
  font-size: 16px;
  cursor: pointer;
  display: flex;
  align-items: center;
  justify-content: center;
  z-index: 10;
}

.modal-hero {
  background-color: #e9ecef;
  height: 180px;
  display: flex;
  align-items: center;
  justify-content: center;
}

.modal-emoji {
  font-size: 72px;
}

.modal-body {
  padding: 24px;
}

.modal-body h2 {
  font-size: 22px;
  font-weight: 800;
  margin: 10px 0 16px 0;
  line-height: 1.3;
}

.modal-addr, .modal-tel {
  font-size: 14px;
  color: #495057;
  margin: 6px 0;
}

.divider {
  border: 0;
  border-top: 1px solid #dee2e6;
  margin: 16px 0;
}

.modal-desc {
  font-size: 15px;
  color: #343a40;
  line-height: 1.6;
}
</style>