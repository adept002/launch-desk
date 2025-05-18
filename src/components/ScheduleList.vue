<template>
  <div class="kanban-scroll"
  ref="scrollContainer"
  @mousedown="startDrag"
  @mousemove="onDrag"
  @mouseup="stopDrag"
  @mouseleave="stopDrag"
  >
    <div class="kanban-row">
      <div class="kanban-column"
      v-for="course in filteredLaunches"
      :key="course.id">
        <div class="column-header">
          <h3 class="column-header__h3">{{ course.name }}</h3>
          <span class="column-header__span"
            v-for="categ in course.categories"
            :key="categ.key"
            :style="{ backgroundColor: categ.color, color: 'white' }"
          >
            {{ categ.name }}
          </span>
        </div>

        <div class="column-body">
          <div v-if="course.groups?.length" class="section groups">
            <strong>Старт групп</strong>
            <ul class="section-li">
              <li class="section-li" v-for="date in getSortedUniqueDates(course.groups)" :key="date">
                <p class="section-date">{{ formatDate(date) }}</p>
                <ul class="section-li time-block">
                  <li v-for="group in sortedGroups(course.groups).filter(g => g.start_date === date)"
                  :key="group.id" class="section-time">
                    {{ group.study_time }}
                  </li>
                </ul>
              </li>
            </ul>
          </div>

          <div v-if="course.open_lessons?.length" class="section lessons">
            <strong>Открытые уроки</strong>
            <ul class="section-li">
              <li class="section-li" v-for="date in getSortedUniqueLessonDates(course.open_lessons)" :key="date" >
                <p class="section-date">{{ formatDate(date) }}</p>
                <ul class="section-li time-block">
                  <li v-for="lesson in sortedLessons(course.open_lessons).filter(l => l.date === date)"
                  :key="lesson.id" class="section-time">
                    {{ lesson.time }}
                  </li>
                </ul>
              </li>
            </ul>
          </div>
        </div>

      </div>
    </div>
  </div>
</template>

<script setup>
defineProps({
  filteredLaunches: Array,
})

import { ref } from 'vue'

const getSortedUniqueDates = (groups) => {
  const sorted = sortedGroups(groups || [])
  const unique = [...new Set(sorted.map(g => g.start_date))]
  return unique
}

const getSortedUniqueLessonDates = (lessons) => {
  const sorted = sortedLessons(lessons || [])
  const unique = [...new Set(sorted.map(l => l.date))]
  return unique
}

const sortedGroups = (groups) =>
  [...groups].sort((a, b) => {
    const dateA = new Date(`${a.start_date}${a.study_time}`)
    const dateB = new Date(`${b.start_date}${b.study_time}`)
    return dateA - dateB
  })

const sortedLessons = (lessons) =>
  [...lessons].sort((a, b) => {
    const dateA = new Date(`${a.date}${a.time}`)
    const dateB = new Date(`${b.date}${b.time}`)
    return dateB - dateA || dateA - dateB 
  })

  const formatDate = (dateStr) => {
  const date = new Date(dateStr)
  const formatter = new Intl.DateTimeFormat('ru-RU', {
    day: 'numeric',
    month: 'long',
    year: 'numeric',
  })
  return formatter.format(date).replace(' г.', '')
}

// =======


const scrollContainer = ref(null)
let isDragging = false
let startX
let scrollLeft

const startDrag = (e) => {
  isDragging = true
  startX = e.pageX - scrollContainer.value.offsetLeft
  scrollLeft = scrollContainer.value.scrollLeft
  scrollContainer.value.style.cursor = 'grabbing'
}

const onDrag = (e) => {
  if (!isDragging) return
  e.preventDefault()
  const x = e.pageX - scrollContainer.value.offsetLeft
  const walk = x - startX
  scrollContainer.value.scrollLeft = scrollLeft - walk
}

const stopDrag = () => {
  isDragging = false
  scrollContainer.value.style.cursor = 'grab'
}

</script>
