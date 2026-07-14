<template>
  <div class="board-container">
    <!-- 검색 및 필터 바 -->
    <div class="filter-bar">
      <div class="search-box">
        <span class="search-icon">🔍</span>
        <input 
          v-model="searchQuery"
          type="text" 
          placeholder="검색어를 입력하세요 (제목, 내용)" 
          class="search-input"
        />
      </div>
      
      <div class="filter-actions">
        <select v-model="sortBy" class="sort-select">
          <option value="latest">최신순</option>
          <option value="views">조회순</option>
        </select>
        <button class="filter-btn">⚙️ 필터</button>
      </div>
    </div>

    <!-- 게시글 목록 -->
    <div class="post-list">
      <div v-for="post in posts" :key="post.id" class="post-item">
        <!-- 이미지 플레이스홀더 -->
        <div class="post-thumbnail">🖼️</div>
        
        <!-- 게시글 정보 -->
        <div class="post-info">
          <div class="post-header">
            <h3 class="post-title">{{ post.title }}</h3>
            <span class="post-views">👁️ {{ post.views }}</span>
          </div>
          <p class="post-summary">{{ post.summary }}</p>
          
          <div class="post-footer">
            <div class="post-meta">
              <span class="author">{{ post.author }}</span>
              <span class="divider">|</span>
              <span>{{ post.date }}</span>
            </div>
            
            <div class="post-buttons">
              <button class="action-btn score">🤍 {{ post.likes }}</button>
              <button class="action-btn">🔖</button>
              <button class="action-btn">🔗</button>
            </div>
          </div>
        </div>
      </div>
    </div>

    <!-- 페이지네이션 & 글쓰기 -->
    <div class="board-footer">
      <div class="pagination">
        <button class="page-arrow">‹</button>
        <button 
          v-for="page in 5" 
          :key="page"
          :class="['page-num', { active: page === 1 }]"
        >
          {{ page }}
        </button>
        <button class="page-arrow">›</button>
      </div>

      <button class="write-btn">글쓰기</button>
    </div>
  </div>
</template>

<script setup>
import { ref } from 'vue'

const searchQuery = ref('')
const sortBy = ref('latest')

const posts = ref([
  {
    id: 1,
    title: '구미 금오산 등산 코스 추천!',
    summary: '초보자도 쉽게 다녀올 수 있는 코스 정리했어요. 경치도 좋고 강력 추천합니다 :)',
    author: '산타는곰',
    date: '2024.07.14',
    views: 325,
    likes: 23
  },
  {
    id: 2,
    title: '구미 인생 맛집 BEST 5',
    summary: '현지인이 자주 가는 찐맛집들만 모아봤습니다!',
    author: '맛집헌터',
    date: '2024.07.13',
    views: 187,
    likes: 15
  },
  {
    id: 3,
    title: '선산 꽃감 축제 다녀왔어요!',
    summary: '곶감도 맛있고 볼거리도 많았던 축제 후기입니다. 주차 정보도 공유해요!',
    author: '경북여행가',
    date: '2024.07.12',
    views: 412,
    likes: 31
  }
])
</script>

<style scoped>
.board-container {
  display: flex;
  flex-direction: column;
  height: 100%;
  flex: 1;
}
.filter-bar {
  display: flex;
  gap: 16px;
  margin-bottom: 24px;
}
.search-box {
  position: relative;
  flex: 1;
}
.search-icon {
  position: absolute;
  left: 16px;
  top: 50%;
  transform: translateY(-50%);
  font-size: 16px;
  color: #868e96;
}
.search-input {
  width: 100%;
  box-sizing: border-box;
  padding: 14px 16px 14px 44px;
  border: 1.5px solid #dee2e6;
  border-radius: 12px;
  font-size: 15px;
  background-color: #f8f9fa;
  outline: none;
  transition: all 0.2s;
}
.search-input:focus {
  background-color: #fff;
  border-color: #868e96;
}
.filter-actions {
  display: flex;
  gap: 10px;
}
.sort-select {
  padding: 0 16px;
  border: 1.5px solid #dee2e6;
  border-radius: 12px;
  font-size: 14px;
  font-weight: 600;
  background-color: #fff;
  outline: none;
}
.filter-btn {
  padding: 0 16px;
  border: 1.5px solid #dee2e6;
  border-radius: 12px;
  font-size: 14px;
  font-weight: 600;
  background-color: #fff;
  cursor: pointer;
  transition: background-color 0.2s;
}
.filter-btn:hover {
  background-color: #f8f9fa;
}
.post-list {
  display: flex;
  flex-direction: column;
  flex: 1;
  gap: 8px;
}
.post-item {
  display: flex;
  gap: 24px;
  padding: 24px 0;
  border-bottom: 1.5px solid #f1f3f5;
}
.post-item:first-child { padding-top: 0; }
.post-item:last-child { border-bottom: none; }

.post-thumbnail {
  width: 120px;
  height: 120px;
  background-color: #f1f3f5;
  border: 1.5px solid #dee2e6;
  border-radius: 12px;
  display: flex;
  align-items: center;
  justify-content: center;
  font-size: 32px;
  color: #adb5bd;
  flex-shrink: 0;
}
.post-info {
  flex: 1;
  min-width: 0;
  display: flex;
  flex-direction: column;
  justify-content: space-between;
}
.post-header {
  display: flex;
  justify-between: space-between;
  align-items: flex-start;
  gap: 12px;
}
.post-title {
  font-size: 18px;
  font-weight: 800;
  color: #111;
  margin: 0;
  white-space: nowrap;
  overflow: hidden;
  text-overflow: ellipsis;
}
.post-views {
  font-size: 13px;
  color: #868e96;
  font-weight: 500;
  flex-shrink: 0;
}
.post-summary {
  font-size: 14px;
  color: #495057;
  margin: 8px 0;
  display: -webkit-box;
  -webkit-line-clamp: 2;
  -webkit-box-orient: vertical;
  overflow: hidden;
  line-height: 1.5;
}
.post-footer {
  display: flex;
  justify-content: space-between;
  align-items: center;
  margin-top: 8px;
}
.post-meta {
  font-size: 13px;
  color: #868e96;
}
.author {
  font-weight: 700;
  color: #495057;
}
.divider {
  margin: 0 8px;
  color: #dee2e6;
}
.post-buttons {
  display: flex;
  gap: 6px;
}
.action-btn {
  background: #fff;
  border: 1.5px solid #dee2e6;
  border-radius: 8px;
  padding: 6px 12px;
  font-size: 13px;
  cursor: pointer;
  color: #495057;
  transition: all 0.2s;
}
.action-btn.score {
  display: flex;
  align-items: center;
  gap: 6px;
  font-weight: 600;
}
.action-btn:hover {
  background-color: #f8f9fa;
  border-color: #868e96;
}
.board-footer {
  margin-top: 24px;
  border-top: 1.5px solid #f1f3f5;
  padding-top: 24px;
}
.pagination {
  display: flex;
  justify-content: center;
  align-items: center;
  gap: 6px;
  margin-bottom: 24px;
}
.page-arrow, .page-num {
  background: none;
  border: none;
  width: 36px;
  height: 36px;
  font-size: 14px;
  font-weight: 600;
  color: #495057;
  border-radius: 8px;
  cursor: pointer;
  display: flex;
  align-items: center;
  justify-content: center;
}
.page-num.active {
  background-color: #495057;
  color: #fff;
}
.page-num:hover:not(.active), .page-arrow:hover {
  background-color: #e9ecef;
}
.write-btn {
  width: 100%;
  padding: 16px 0;
  background-color: #f1f3f5;
  border: none;
  border-radius: 12px;
  font-size: 16px;
  font-weight: 800;
  color: #212529;
  cursor: pointer;
  transition: background-color 0.2s;
}
.write-btn:hover {
  background-color: #e2e6ea;
}

/* 📱 모바일 최적화: 게시판을 콤팩트하고 슬림하게 조정 */
@media (max-width: 640px) {
  .filter-bar {
    flex-direction: row; /* 검색창과 필터 한 줄 유지 */
    gap: 8px;
    margin-bottom: 12px;
  }
  .search-input {
    padding: 8px 10px 8px 30px;
    font-size: 12px;
    border-radius: 8px;
  }
  .search-icon {
    left: 10px;
    font-size: 12px;
  }
  .sort-select, .filter-btn {
    padding: 0 8px;
    font-size: 11px;
    border-radius: 8px;
  }
  .post-item {
    flex-direction: row; /* 세로 누적을 방지하고 콤팩트한 가로 배치 유지 */
    gap: 12px;
    padding: 12px 0;
  }
  .post-thumbnail {
    width: 64px; /* 썸네일 크기 대폭 축소 */
    height: 64px;
    font-size: 20px;
    border-radius: 8px;
  }
  .post-title {
    font-size: 14px; /* 글 제목 축소 */
  }
  .post-views {
    font-size: 10px;
  }
  .post-summary {
    font-size: 11px; /* 글 요약 축소 및 여백 보정 */
    margin: 4px 0;
    -webkit-line-clamp: 1; /* 글 요약을 1줄만 보이도록 하여 영역 절약 */
  }
  .post-meta {
    font-size: 10px;
  }
  .action-btn {
    padding: 3px 6px; /* 버튼 패딩 극소화 */
    font-size: 10px;
    border-radius: 4px;
  }
  .action-btn.score {
    gap: 3px;
  }
  .board-footer {
    margin-top: 12px;
    padding-top: 12px;
  }
  .pagination {
    margin-bottom: 12px;
    gap: 2px;
  }
  .page-arrow, .page-num {
    width: 28px;
    height: 28px;
    font-size: 11px;
    border-radius: 6px;
  }
  .write-btn {
    padding: 10px 0;
    font-size: 13px;
    border-radius: 8px;
  }
}
</style>