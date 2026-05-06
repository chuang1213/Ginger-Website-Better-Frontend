<template>
  <div class="about-page">
    <n-card class="about-card" bordered="false">
      <template #header>
        <div class="card-header">
          <n-icon size="24" color="#18a058">
            <InformationIcon />
          </n-icon>
          <h2>About Ginger Drive</h2>
        </div>
      </template>

      <div class="about-content">
        <p class="description">
          Project Ginger-Rush is aiming to serve as a multiple usage service of BMS ecosystem.
        </p>

        <n-divider />

        <h3>Features</h3>
        <n-list class="feature-list">
          <n-list-item class="feature-item">
            <template #prefix>
              <n-icon size="20" color="#18a058">
                <DownloadIcon />
              </n-icon>
            </template>
            BMS package download website
          </n-list-item>
          <n-list-item class="feature-item">
            <template #prefix>
              <n-icon size="20" color="#18a058">
                <CloudIcon />
              </n-icon>
            </template>
            Cloud storage integration
          </n-list-item>
          <n-list-item class="feature-item">
            <template #prefix>
              <n-icon size="20" color="#18a058">
                <SearchIcon />
              </n-icon>
            </template>
            Advanced search capabilities
          </n-list-item>
        </n-list>

        <n-divider />

        <p class="status">More features coming soon...</p>
      </div>
    </n-card>

    <n-card class="contributors-card" bordered="false">
      <template #header>
        <div class="card-header">
          <n-icon size="24" color="#18a058">
            <PeopleIcon />
          </n-icon>
          <h2>Contributors</h2>
        </div>
      </template>

      <n-list class="contributor-list">
        <n-list-item class="contributor-item">
          <n-flex align="center">
            <n-avatar round size="large" style="background-color: #18a058">
              C
            </n-avatar>
            <div class="contributor-info">
              <span class="contributor-name">Catizard</span>
              <span class="contributor-role">Developer</span>
            </div>
            <n-button text size="large" @click="openGithub">
              <template #icon>
                <n-icon>
                  <GithubIcon />
                </n-icon>
              </template>
            </n-button>
          </n-flex>
        </n-list-item>
      </n-list>
    </n-card>
  </div>
</template>

<script setup lang="ts">
import {
  InformationCircleOutline as InformationIcon,
  DownloadOutline as DownloadIcon,
  CloudOutline as CloudIcon,
  SearchOutline as SearchIcon,
  PeopleOutline as PeopleIcon,
  LogoGithub as GithubIcon
} from '@vicons/ionicons5'

function openGithub() {
  window.open('https://github.com', '_blank')
}
</script>

<style scoped>
.about-page {
  max-width: 900px;
  margin: 0 auto;
}

.about-card,
.contributors-card {
  background: #ffffff;
  border-radius: 12px;
  box-shadow: 0 1px 3px rgba(0, 0, 0, 0.08);
  margin-bottom: 20px;
}

.card-header {
  display: flex;
  align-items: center;
  gap: 12px;
}

.card-header h2 {
  margin: 0;
  font-size: 18px;
  font-weight: 600;
  color: #1d2129;
}

.about-content {
  padding: 8px 0;
}

.description {
  font-size: 15px;
  color: #4e5969;
  line-height: 1.6;
}

h3 {
  font-size: 15px;
  font-weight: 600;
  color: #1d2129;
  margin-bottom: 12px;
}

.feature-list {
  margin: 12px 0;
}

.feature-item {
  padding: 10px 12px;
  border-radius: 8px;
  transition: background 0.2s;
}

.feature-item:hover {
  background: #f7f8fa;
}

.status {
  color: #86909c;
  font-style: italic;
}

.contributor-list {
  padding: 8px 0;
}

.contributor-item {
  padding: 12px;
  border-radius: 8px;
  transition: background 0.2s;
}

.contributor-item:hover {
  background: #f7f8fa;
}

.contributor-info {
  display: flex;
  flex-direction: column;
}

.contributor-name {
  font-weight: 600;
  color: #1d2129;
}

.contributor-role {
  font-size: 12px;
  color: #86909c;
}

:deep(.n-divider) {
  margin: 16px 0;
}

@media (prefers-color-scheme: dark) {
  .about-card,
  .contributors-card {
    background: #232323;
    box-shadow: 0 1px 3px rgba(0, 0, 0, 0.3);
  }

  .card-header h2 {
    color: #e5e6eb;
  }

  .description {
    color: #c9cdd4;
  }

  h3 {
    color: #e5e6eb;
  }

  .feature-item:hover {
    background: #2a2a2a;
  }

  .status {
    color: #a0a0a0;
  }

  .contributor-item:hover {
    background: #2a2a2a;
  }

  .contributor-name {
    color: #e5e6eb;
  }
}
</style>
