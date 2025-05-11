---
title: 독서리스트
icon: fas fa-book
order: 5
---

## 📚 나의 독서 기록

이 페이지에서는 2025년 간 제가 읽은 책, 현재 읽고 있는 책, 그리고 앞으로 읽고 싶은 책들을 소개합니다.

{% include reading-progress.html %}

### 📖 완독한 책

<div class="book-section">
  {% for category in site.data.completed_books %}
  <div class="book-category">
    <h4>📚 {{ category.category }}</h4>
    <div class="book-list">
      {% for book in category.books %}
      <div class="book-item">
        <div class="book-title">{{ book.title }}</div>
        <div class="book-meta">
          <span class="book-author">{{ book.author }}</span>
          <span class="book-rating">
            {% for i in (1..book.rating) %}⭐{% endfor %}
          </span>
          <span class="book-date">{{ book.date }}</span>
        </div>
        <div class="book-tags">
          {% for tag in book.tags %}
          <span class="book-tag">{{ tag }}</span>
          {% endfor %}
        </div>
      </div>
      {% endfor %}
    </div>
  </div>
  {% endfor %}
</div>

### 📖 읽고 있는 책

<div class="reading-now">
  {% for book in site.data.current_books %}
  <div class="book-item current">
    <div class="book-title">{{ book.title }}</div>
    <div class="book-meta">
      <span class="book-author">{{ book.author }}</span>
    </div>
    <div class="progress-container">
      <div class="progress-bar">
        <div class="progress" style="width: {{ book.progress }}%"></div>
      </div>
      <span class="progress-text">{{ book.progress }}%</span>
    </div>
    <div class="book-tags">
      {% for tag in book.tags %}
      <span class="book-tag">{{ tag }}</span>
      {% endfor %}
    </div>
  </div>
  {% endfor %}
</div>

### 📖 읽을 예정인 책

<div class="book-section to-read">
  <div class="book-list">
    {% for book in site.data.to_read_books %}
    <div class="book-item">
      <div class="book-title">{{ book.title }}</div>
      <div class="book-meta">
        <span class="book-author">{{ book.author }}</span>
      </div>
      <div class="book-tags">
        {% for tag in book.tags %}
        <span class="book-tag">{{ tag }}</span>
        {% endfor %}
      </div>
    </div>
    {% endfor %}
  </div>
</div>

<style>
.book-section {
  margin-bottom: 2rem;
}

.book-category {
  margin-bottom: 1.5rem;
}

.book-list {
  display: grid;
  grid-template-columns: repeat(auto-fill, minmax(300px, 1fr));
  gap: 1.5rem;
}

.book-item {
  padding: 1rem;
  border-radius: 0.5rem;
  background-color: var(--card-bg);
  border: 1px solid var(--border-color);
  transition: transform 0.2s ease, box-shadow 0.2s ease;
  box-shadow: 0 2px 5px rgba(0, 0, 0, 0.05);
}

.book-category h4 {
  padding-bottom: 0.5rem;
  border-bottom: 2px solid var(--border-color);
  margin-bottom: 1rem;
}

/* 카테고리별 색상 구분 추가 */
.book-category:nth-child(odd) .book-item {
  border-left: 4px solid #4caf50;
}

.book-category:nth-child(even) .book-item {
  border-left: 4px solid #2196f3;
}

.book-item:hover {
  transform: translateY(-3px);
  box-shadow: 0 5px 15px rgba(0, 0, 0, 0.1);
   border-color: var(--link-color);
}

.book-title {
  font-weight: bold;
  font-size: 1.1rem;
  margin-bottom: 0.5rem;
}

.book-meta {
  display: flex;
  flex-wrap: wrap;
  justify-content: space-between;
  margin-bottom: 0.5rem;
  font-size: 0.9rem;
  color: var(--text-muted-color);
  padding-bottom: 0.5rem;
  border-bottom: 1px dashed var(--border-color);
}

.book-author {
  flex: 1;
  min-width: 60%;
}

.book-tags {
  margin-top: 0.5rem;
}

.book-tag {
  display: inline-block;
  padding: 0.2rem 0.5rem;
  margin-right: 0.3rem;
  margin-bottom: 0.3rem;
  border-radius: 0.3rem;
  background-color: var(--tag-bg);
  color: var(--tag-color);
  font-size: 0.8rem;
}

/* 읽고 있는 책 섹션 스타일링 */
.reading-now {
  display: grid;
  grid-template-columns: repeat(auto-fill, minmax(300px, 1fr));
  gap: 1.5rem;
  margin-bottom: 2rem;
}

.reading-now .book-item {
  background-color: rgba(255, 193, 7, 0.1);
  border-color: rgba(255, 193, 7, 0.3);
  border-left: 4px solid #ff9800;
}

.to-read .book-item {
  background-color: rgba(33, 150, 243, 0.1);
  border-color: rgba(33, 150, 243, 0.3);
}

/* 진행률 표시 영역 수정 */
.progress-container {
  display: flex;
  align-items: center;
  margin: 0.5rem 0;
  gap: 10px;
}

.progress-bar {
  flex: 1;
  height: 0.5rem;
  background-color: var(--border-color);
  border-radius: 0.25rem;
  overflow: hidden;
}

.progress {
  height: 100%;
  background-color: #4caf50;
}

.progress-text {
  font-size: 0.85rem;
  font-weight: bold;
  color: #4caf50;
  min-width: 45px;
  text-align: right;
}

/* 다크 모드 대응 */
[data-mode="dark"] .reading-now .book-item {
  background-color: rgba(255, 193, 7, 0.15);
}

[data-mode="dark"] .to-read .book-item {
  background-color: rgba(33, 150, 243, 0.15);
}

/* 반응형 디자인 */
@media (max-width: 768px) {
  .reading-now, .book-list {
    grid-template-columns: 1fr;
  }
}
</style>
