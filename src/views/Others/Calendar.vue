<template>
  <AdminLayout>
    <PageBreadcrumb :pageTitle="currentPageTitle" />
    <div class="rounded-2xl border border-gray-200 bg-white dark:border-gray-800 dark:bg-white/[0.03]">
      <div class="custom-calendar">
        <FullCalendar ref="calendarRef" class="min-h-screen" :options="calendarOptions" />
      </div>

      <!-- Modal -->
      <Modal v-if="isOpen" @close="closeModal = false">
        <template #body>
          <div
            class="no-scrollbar relative w-full max-w-[700px] overflow-y-auto rounded-3xl bg-white p-4 dark:bg-gray-900 lg:p-11">
            <h5 class="mb-2 font-semibold text-gray-800 modal-title text-theme-xl dark:text-white/90 lg:text-2xl">
              {{ selectedEvent ? 'Editar Consulta' : 'Agendar Consulta' }}
            </h5>
            <p class="text-sm text-gray-500 dark:text-gray-400">
              Preencha as Informações para agendar a consulta
            </p>

            <div class="mt-8">
              <div>
                <label class="mb-1.5 block text-sm font-medium text-gray-700 dark:text-gray-400">
                  Nome do Paciente
                </label>
                <input v-model="eventTitle" type="text"
                  class="dark:bg-dark-900 h-11 w-full rounded-lg border border-gray-300 bg-transparent px-4 py-2.5 text-sm text-gray-800 shadow-theme-xs placeholder:text-gray-400 focus:border-brand-300 focus:outline-hidden focus:ring-3 focus:ring-brand-500/10 dark:border-gray-700 dark:bg-gray-900 dark:text-white/90 dark:placeholder:text-white/30 dark:focus:border-brand-800" />
              </div>

              <div class="mt-6">
                <label class="block mb-4 text-sm font-medium text-gray-700 dark:text-gray-400">
                  Event Color
                </label>
                <div class="flex flex-wrap items-center gap-4 sm:gap-5">
                  <div v-for="(value, key) in calendarsEvents" :key="key" class="n-chk">
                    <div :class="`form-check form-check-${value} form-check-inline`">
                      <label class="flex items-center text-sm text-gray-700 form-check-label dark:text-gray-400"
                        :for="`modal${key}`">
                        <span class="relative">
                          <input type="radio" :name="'event-level'" :value="key" :id="`modal${key}`"
                            v-model="eventLevel" class="sr-only form-check-input" />
                          <span
                            class="flex items-center justify-center w-5 h-5 mr-2 border border-gray-300 rounded-full box dark:border-gray-700">
                            <span class="w-2 h-2 bg-white rounded-full dark:bg-transparent"></span>
                          </span>
                        </span>
                        {{ key }}
                      </label>
                    </div>
                  </div>
                </div>
              </div>

              <div class="mt-6">
                <label class="mb-1.5 block text-sm font-medium text-gray-700 dark:text-gray-400">
                  Data
                </label>
                <input v-model="eventStartDate" type="date"
                  class="dark:bg-dark-900 h-11 w-full appearance-none rounded-lg border border-gray-300 bg-transparent bg-none px-4 py-2.5 pl-4 pr-11 text-sm text-gray-800 shadow-theme-xs placeholder:text-gray-400 focus:border-brand-300 focus:outline-hidden focus:ring-3 focus:ring-brand-500/10 dark:border-gray-700 dark:bg-gray-900 dark:text-white/90 dark:placeholder:text-white/30 dark:focus:border-brand-800" />
              </div>
            </div>

            <div class="flex items-center gap-3 mt-6 modal-footer sm:justify-end">
              <button @click="closeModal"
                class="flex w-full justify-center rounded-lg border border-gray-300 bg-white px-4 py-2.5 text-sm font-medium text-gray-700 hover:bg-gray-50 dark:border-gray-700 dark:bg-gray-800 dark:text-gray-400 dark:hover:bg-white/[0.03] sm:w-auto">
                Fechar
              </button>

              <button @click="handleAddOrUpdateEvent"
                class="btn btn-success btn-update-event flex w-full justify-center rounded-lg bg-brand-500 px-4 py-2.5 text-sm font-medium text-white hover:bg-brand-600 sm:w-auto">
                {{ selectedEvent ? 'Atualizar Informações' : 'Agendar Consulta' }}
              </button>
              <button v-if="selectedEvent" @click="handleDeleteEvent"
                class="flex w-full justify-center rounded-lg border border-error-500 bg-error-500 px-4 py-2.5 text-sm font-medium text-white hover:bg-error-600 sm:w-auto">
                Excluir Consulta
              </button>
            </div>
          </div>
        </template>
      </Modal>

    </div>
  </AdminLayout>
</template>

<script setup>
import AdminLayout from '@/components/layout/AdminLayout.vue'
import PageBreadcrumb from '@/components/common/PageBreadcrumb.vue'
import ptBrLocale from '@fullcalendar/core/locales/pt-br'
const currentPageTitle = ref('Agenda')
import { ref, reactive, onMounted } from 'vue'
import FullCalendar from '@fullcalendar/vue3'
import dayGridPlugin from '@fullcalendar/daygrid'
import timeGridPlugin from '@fullcalendar/timegrid'
import interactionPlugin from '@fullcalendar/interaction'
import Modal from '@/components/profile/Modal.vue'

const calendarRef = ref(null)
const isOpen = ref(false)
const selectedEvent = ref(null)
const eventTitle = ref('')
const eventStartDate = ref('')
const eventEndDate = ref('')
const eventLevel = ref('')
const events = ref([])

const calendarsEvents = reactive({
  Crianca: 'danger',
  Adolescente: 'success',
  Adulto: 'primary',
  Idoso: 'warning',
})

const colorMap = {
  Crianca: 'danger',
  Adolescente: 'success',
  Adulto: 'primary',
  Idoso: 'warning'
}


onMounted(() => {
  const today = new Date()

  const thisMonth = today
  const prevMonth = new Date(today.getFullYear(), today.getMonth() - 1, 1)
  const nextMonth = new Date(today.getFullYear(), today.getMonth() + 1, 1)

  const toDate = d => d.toISOString().split('T')[0]

  events.value = [
    // Mês passado
    {
      id: '1',
      title: 'Laura Martins',
      start: toDate(new Date(prevMonth.getFullYear(), prevMonth.getMonth(), 5)),
      extendedProps: { calendar: 'Danger' },
    },
    {
      id: '2',
      title: 'Felipe Andrade',
      start: toDate(new Date(prevMonth.getFullYear(), prevMonth.getMonth(), 14)),
      extendedProps: { calendar: 'Success' },
    },

    // Mês atual
    {
      id: '3',
      title: 'Sofia Ribeiro',
      start: toDate(new Date(thisMonth.getFullYear(), thisMonth.getMonth(), 3)),
      extendedProps: { calendar: 'Primary' },
    },
    {
      id: '4',
      title: 'Gabriel Costa',
      start: toDate(new Date(thisMonth.getFullYear(), thisMonth.getMonth(), 10)),
      extendedProps: { calendar: 'Info' },
    },
    {
      id: '5',
      title: 'Ana Júlia',
      start: toDate(new Date(thisMonth.getFullYear(), thisMonth.getMonth(), 18)),
      end: toDate(new Date(thisMonth.getFullYear(), thisMonth.getMonth(), 20)),
      extendedProps: { calendar: 'Warning' },
    },

    // Mês que vem
    {
      id: '6',
      title: 'Ricardo Almeida',
      start: toDate(new Date(nextMonth.getFullYear(), nextMonth.getMonth(), 4)),
      extendedProps: { calendar: 'Success' },
    },
    {
      id: '7',
      title: 'Carolina Lopes',
      start: toDate(new Date(nextMonth.getFullYear(), nextMonth.getMonth(), 12)),
      extendedProps: { calendar: 'Danger' },
    },
    {
      id: '8',
      title: 'Thiago Pires',
      start: toDate(new Date(nextMonth.getFullYear(), nextMonth.getMonth(), 22)),
      end: toDate(new Date(nextMonth.getFullYear(), nextMonth.getMonth(), 25)),
      extendedProps: { calendar: 'Primary' },
    },
  ]
})


const openModal = () => {
  isOpen.value = true
}

const closeModal = () => {
  isOpen.value = false
  resetModalFields()
}

const resetModalFields = () => {
  eventTitle.value = ''
  eventStartDate.value = ''
  eventEndDate.value = ''
  eventLevel.value = ''
  selectedEvent.value = null
}

const handleDateSelect = (selectInfo) => {
  resetModalFields()
  eventStartDate.value = selectInfo.startStr
  eventEndDate.value = selectInfo.endStr || selectInfo.startStr
  openModal()
}

const handleEventClick = (clickInfo) => {
  const event = clickInfo.event
  selectedEvent.value = event
  eventTitle.value = event.title
  eventStartDate.value = event.start?.toISOString().split('T')[0] || ''
  eventEndDate.value = event.end?.toISOString().split('T')[0] || ''
  eventLevel.value = event.extendedProps.calendar
  openModal()
}

const handleAddOrUpdateEvent = () => {
  if (selectedEvent.value) {
    // Update existing event
    events.value = events.value.map((event) =>
      event.id === selectedEvent.value.id
        ? {
          ...event,
          title: eventTitle.value,
          start: eventStartDate.value,
          end: eventEndDate.value,
          extendedProps: { calendar: eventLevel.value },
        }
        : event,
    )
  } else {
    // Add new event
    const newEvent = {
      id: Date.now().toString(),
      title: eventTitle.value,
      start: eventStartDate.value,
      end: eventEndDate.value,
      allDay: true,
      extendedProps: { calendar: eventLevel.value },
    }
    events.value.push(newEvent)
  }
  closeModal()
}
const handleDeleteEvent = () => {
  if (selectedEvent.value) {
    events.value = events.value.filter((event) => event.id !== selectedEvent.value.id)
    closeModal()
  }
}

const renderEventContent = (eventInfo) => {
  const tipo = eventInfo.event.extendedProps.calendar
  const realColor = colorMap[tipo] || 'primary'
  const colorClass = `fc-bg-${realColor}`

  return {
    html: `
      <div class="event-fc-color flex fc-event-main ${colorClass} p-1 rounded-sm">
        <div class="fc-daygrid-event-dot"></div>
        <div class="fc-event-time">${eventInfo.timeText}</div>
        <div class="fc-event-title">${eventInfo.event.title}</div>
      </div>
    `,
  }
}


const calendarOptions = reactive({
  plugins: [dayGridPlugin, timeGridPlugin, interactionPlugin],
  locale: ptBrLocale,
  initialView: 'dayGridMonth',
  headerToolbar: {
    left: 'prev,next addEventButton',
    center: 'title',
    right: 'dayGridMonth,timeGridWeek,timeGridDay',
  },
  buttonText: {
    month: 'Mês',
    week: 'Semana',
    day: 'Dia',
    today: 'Hoje',
  },
  events: events,
  selectable: true,
  select: handleDateSelect,
  eventClick: handleEventClick,
  eventContent: renderEventContent,
  customButtons: {
    addEventButton: {
      text: 'Agendar Consulta +',
      click: openModal,
    },
  },
})

</script>
