<template>
  <div class="dashboard-container">
    <!-- Sidebar -->
    <aside class="sidebar">
      <div class="sidebar-header">
        <h2 class="logo">
          <span class="logo-icon">N</span>
          extSkill
        </h2>
      </div>
      
      <nav class="sidebar-nav">
        <ul>
          <li :class="{ active: activeMenu === 'dashboard' }">
            <a href="#" @click.prevent="setActiveMenu('dashboard')">
              <i class="icon-dashboard">📊</i>
              Dashboard
            </a>
          </li>
          <li :class="{ active: activeMenu === 'attendance' }">
            <a href="#" @click.prevent="setActiveMenu('attendance')">
              <i class="icon-attendance">📅</i>
              Kehadiran Hari ini
            </a>
          </li>
          <li :class="{ active: activeMenu === 'payroll' }">
            <a href="#" @click.prevent="setActiveMenu('payroll')">
              <i class="icon-payroll">💰</i>
              penggajian
            </a>
          </li>
          <li :class="{ active: activeMenu === 'profile' }">
            <a href="#" @click.prevent="setActiveMenu('profile')">
              <i class="icon-profile">👤</i>
              profil
            </a>
          </li>
        </ul>
      </nav>
    </aside>

    <!-- Main Content -->
    <main class="main-content">
      <!-- Header -->
      <header class="dashboard-header">
        <div class="header-info">
          <div class="date-time">
            <span class="notification-icon">🔔</span>
            <span class="date">{{ currentDate }}</span>
            <span class="time">{{ currentTime }}</span>
          </div>
        </div>
      </header>

      <!-- Dashboard Content -->
      <div class="dashboard-content">
        <div class="content-header">
          <h1>Dashboard karyawan</h1>
          <p>Selamat datang kembali! Berikut ringkasan kehadiran Anda hari ini.</p>
        </div>

        <!-- Status Cards -->
        <div class="status-cards">
          <div class="card status-card hadir">
            <div class="card-content">
              <h3>Status Hari ini</h3>
              <div class="status-display">
                <span class="status-text">Hadir</span>
                <div class="status-icon">✓</div>
              </div>
            </div>
          </div>
          
          <div class="card status-card working-hours">
            <div class="card-content">
              <h3>Jam Kerja</h3>
              <div class="hours-display">
                <span class="hours">{{ workingHours }}</span>
                <span class="hours-label">Hari ini</span>
              </div>
            </div>
          </div>
        </div>

        <!-- Content Grid -->
        <div class="content-grid">
          <!-- Upcoming Events -->
          <div class="card events-card">
            <div class="card-header">
              <h3>📅 acara yang akan datang</h3>
            </div>
            <div class="card-body">
              <div class="event-list">
                <div v-for="event in upcomingEvents" :key="event.id" class="event-item">
                  <div class="event-info">
                    <h4>{{ event.title }}</h4>
                    <p class="event-time">{{ event.time }}</p>
                  </div>
                </div>
              </div>
            </div>
          </div>

          <!-- Pending Requests -->
          <div class="card requests-card">
            <div class="card-header">
              <h3>permintaan tertunda</h3>
            </div>
            <div class="card-body">
              <div v-for="request in pendingRequests" :key="request.id" class="request-item">
                <div class="request-info">
                  <h4>{{ request.title }}</h4>
                  <p class="request-date">{{ request.date }}</p>
                </div>
                <span class="request-status">{{ request.status }}</span>
              </div>
            </div>
          </div>
        </div>

        <!-- Attendance History -->
        <div class="card attendance-history">
          <div class="card-header">
            <h3>Riwayat Kehadiran Terkini</h3>
          </div>
          <div class="card-body">
            <div class="table-container">
              <table class="attendance-table">
                <thead>
                  <tr>
                    <th>tanggal</th>
                    <th>check in</th>
                    <th>check out</th>
                    <th>jam</th>
                    <th>status</th>
                  </tr>
                </thead>
                <tbody>
                  <tr v-for="record in attendanceHistory" :key="record.id">
                    <td>{{ record.date }}</td>
                    <td>{{ record.checkIn }}</td>
                    <td>{{ record.checkOut }}</td>
                    <td>{{ record.hours }}</td>
                    <td>
                      <span class="status-badge" :class="record.status.toLowerCase()">
                        {{ record.status }}
                      </span>
                    </td>
                  </tr>
                </tbody>
              </table>
            </div>
            <div class="table-footer">
              <a href="#" class="view-more">lihat lebih lanjut →</a>
            </div>
          </div>
        </div>
      </div>
    </main>
  </div>
</template>

<script>
export default {
  name: 'Dashboard',
  props: {
    user: {
      type: Object,
      required: true
    }
  },
  data() {
    return {
      activeMenu: 'dashboard',
      currentDate: '',
      currentTime: '',
      workingHours: '7h 32m',
      upcomingEvents: [
        { id: 1, title: 'team meeting', time: 'besok, 09.14 AM' },
        { id: 2, title: 'review project', time: 'jue, 14, 09.11 AM' },
        { id: 3, title: 'Penilaian Bulanan', time: 'Rab, 14, 09.14 AM' }
      ],
      pendingRequests: [
        { 
          id: 1, 
          title: 'urusan mendadak', 
          date: 'Jan 30-22, 2026', 
          status: 'tertunda' 
        }
      ],
      attendanceHistory: [
        { 
          id: 1, 
          date: 'Dec 15, 2024', 
          checkIn: '08:45 AM', 
          checkOut: '-', 
          hours: '-', 
          status: 'hadir' 
        },
        { 
          id: 2, 
          date: 'Dec 14, 2024', 
          checkIn: '08:30 AM', 
          checkOut: '05:45 PM', 
          hours: '9h 15m', 
          status: 'hadir' 
        },
        { 
          id: 3, 
          date: 'Dec 13, 2024', 
          checkIn: '09:00 AM', 
          checkOut: '06:00 PM', 
          hours: '8h 00m', 
          status: 'hadir' 
        }
      ]
    }
  },
  mounted() {
    this.updateDateTime()
    this.dateTimeInterval = setInterval(this.updateDateTime, 1000)
  },
  beforeUnmount() {
    if (this.dateTimeInterval) {
      clearInterval(this.dateTimeInterval)
    }
  },
  methods: {
    setActiveMenu(menu) {
      this.activeMenu = menu
      // Handle menu navigation logic here
    },
    updateDateTime() {
      const now = new Date()
      this.currentDate = now.toLocaleDateString('id-ID', { 
        weekday: 'long', 
        day: 'numeric', 
        month: 'long', 
        year: 'numeric' 
      })
      this.currentTime = now.toLocaleTimeString('id-ID', { 
        hour: '2-digit', 
        minute: '2-digit' 
      })
    }
  }
}
</script>

<style scoped>
.dashboard-container {
  display: flex;
  min-height: 100vh;
  background-color: #f5f7fa;
  font-family: 'Inter', -apple-system, BlinkMacSystemFont, sans-serif;
}

/* Sidebar */
.sidebar {
  width: 280px;
  background: linear-gradient(135deg, #6b46c1 0%, #9333ea 100%);
  color: white;
  padding: 2rem 0;
  position: fixed;
  height: 100vh;
  overflow-y: auto;
}

.sidebar-header {
  padding: 0 2rem 2rem;
  border-bottom: 1px solid rgba(255, 255, 255, 0.1);
}

.logo {
  font-size: 1.5rem;
  font-weight: 700;
  margin: 0;
  display: flex;
  align-items: center;
  gap: 0.5rem;
}

.logo-icon {
  width: 32px;
  height: 32px;
  background: rgba(255, 255, 255, 0.2);
  border-radius: 8px;
  display: flex;
  align-items: center;
  justify-content: center;
  font-weight: 900;
}

.sidebar-nav {
  padding: 2rem 0;
}

.sidebar-nav ul {
  list-style: none;
  margin: 0;
  padding: 0;
}

.sidebar-nav li {
  margin-bottom: 0.5rem;
}

.sidebar-nav a {
  color: rgba(255, 255, 255, 0.8);
  text-decoration: none;
  padding: 1rem 2rem;
  display: flex;
  align-items: center;
  gap: 1rem;
  transition: all 0.2s;
  border-right: 3px solid transparent;
}

.sidebar-nav li.active a,
.sidebar-nav a:hover {
  color: white;
  background: rgba(255, 255, 255, 0.1);
  border-right-color: white;
}

.sidebar-nav .icon-dashboard,
.sidebar-nav .icon-attendance,
.sidebar-nav .icon-payroll,
.sidebar-nav .icon-profile {
  font-size: 1.2rem;
}

/* Main Content */
.main-content {
  flex: 1;
  margin-left: 280px;
  padding: 0;
}

.dashboard-header {
  background: white;
  padding: 1.5rem 2rem;
  border-bottom: 1px solid #e5e7eb;
  display: flex;
  justify-content: space-between;
  align-items: center;
}

.header-info {
  display: flex;
  align-items: center;
  gap: 1rem;
}

.date-time {
  display: flex;
  align-items: center;
  gap: 1rem;
  font-size: 0.9rem;
  color: #6b7280;
}

.notification-icon {
  font-size: 1.2rem;
}

/* Dashboard Content */
.dashboard-content {
  padding: 2rem;
}

.content-header {
  margin-bottom: 2rem;
}

.content-header h1 {
  font-size: 1.875rem;
  font-weight: 700;
  color: #1f2937;
  margin: 0 0 0.5rem 0;
}

.content-header p {
  color: #6b7280;
  margin: 0;
  font-size: 1rem;
}

/* Status Cards */
.status-cards {
  display: grid;
  grid-template-columns: repeat(auto-fit, minmax(300px, 1fr));
  gap: 1.5rem;
  margin-bottom: 2rem;
}

.status-card {
  padding: 1.5rem;
  border-radius: 16px;
  color: white;
  position: relative;
  overflow: hidden;
}

.status-card.hadir {
  background: linear-gradient(135deg, #10b981 0%, #059669 100%);
}

.status-card.working-hours {
  background: linear-gradient(135deg, #8b5cf6 0%, #7c3aed 100%);
}

.status-card h3 {
  font-size: 0.9rem;
  opacity: 0.9;
  margin: 0 0 1rem 0;
  font-weight: 500;
}

.status-display,
.hours-display {
  display: flex;
  justify-content: space-between;
  align-items: center;
}

.status-text,
.hours {
  font-size: 1.5rem;
  font-weight: 700;
}

.hours-label {
  font-size: 0.9rem;
  opacity: 0.9;
}

.status-icon {
  width: 40px;
  height: 40px;
  background: rgba(255, 255, 255, 0.2);
  border-radius: 50%;
  display: flex;
  align-items: center;
  justify-content: center;
  font-size: 1.2rem;
  font-weight: bold;
}

/* Content Grid */
.content-grid {
  display: grid;
  grid-template-columns: 1fr 1fr;
  gap: 1.5rem;
  margin-bottom: 2rem;
}

/* Card Styles */
.card {
  background: white;
  border-radius: 12px;
  box-shadow: 0 1px 3px rgba(0, 0, 0, 0.1);
  overflow: hidden;
}

.card-header {
  padding: 1.5rem 1.5rem 1rem;
  border-bottom: 1px solid #f3f4f6;
}

.card-header h3 {
  margin: 0;
  font-size: 1rem;
  font-weight: 600;
  color: #374151;
}

.card-body {
  padding: 1rem 1.5rem 1.5rem;
}

/* Events */
.event-item {
  padding: 0.75rem 0;
  border-bottom: 1px solid #f3f4f6;
}

.event-item:last-child {
  border-bottom: none;
}

.event-info h4 {
  margin: 0 0 0.25rem 0;
  font-size: 0.9rem;
  font-weight: 600;
  color: #1f2937;
}

.event-time {
  margin: 0;
  font-size: 0.8rem;
  color: #6b7280;
}

/* Requests */
.request-item {
  display: flex;
  justify-content: space-between;
  align-items: center;
  padding: 0.75rem 0;
  border-bottom: 1px solid #f3f4f6;
}

.request-item:last-child {
  border-bottom: none;
}

.request-info h4 {
  margin: 0 0 0.25rem 0;
  font-size: 0.9rem;
  font-weight: 600;
  color: #1f2937;
}

.request-date {
  margin: 0;
  font-size: 0.8rem;
  color: #6b7280;
}

.request-status {
  background: #fef3c7;
  color: #d97706;
  padding: 0.25rem 0.75rem;
  border-radius: 9999px;
  font-size: 0.8rem;
  font-weight: 500;
}

/* Attendance Table */
.attendance-history {
  grid-column: 1 / -1;
}

.table-container {
  overflow-x: auto;
}

.attendance-table {
  width: 100%;
  border-collapse: collapse;
}

.attendance-table th {
  text-align: left;
  padding: 1rem;
  border-bottom: 2px solid #f3f4f6;
  font-weight: 600;
  color: #374151;
  font-size: 0.9rem;
}

.attendance-table td {
  padding: 1rem;
  border-bottom: 1px solid #f3f4f6;
  font-size: 0.9rem;
  color: #6b7280;
}

.status-badge {
  background: #dcfce7;
  color: #166534;
  padding: 0.25rem 0.75rem;
  border-radius: 9999px;
  font-size: 0.8rem;
  font-weight: 500;
}

.status-badge.hadir {
  background: #dcfce7;
  color: #166534;
}

.table-footer {
  padding: 1rem 1.5rem;
  text-align: right;
  border-top: 1px solid #f3f4f6;
}

.view-more {
  color: #6366f1;
  text-decoration: none;
  font-size: 0.9rem;
  font-weight: 500;
}

.view-more:hover {
  text-decoration: underline;
}

/* Responsive */
@media (max-width: 768px) {
  .sidebar {
    width: 240px;
  }
  
  .main-content {
    margin-left: 240px;
  }
  
  .content-grid {
    grid-template-columns: 1fr;
  }
  
  .status-cards {
    grid-template-columns: 1fr;
  }
  
  .dashboard-content {
    padding: 1rem;
  }
}

@media (max-width: 640px) {
  .sidebar {
    transform: translateX(-100%);
    transition: transform 0.3s;
  }
  
  .main-content {
    margin-left: 0;
  }
  
  .dashboard-header {
    padding: 1rem;
  }
}
</style>
