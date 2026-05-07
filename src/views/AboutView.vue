<template>
  <n-card title="Hello">
    Project Ginger-Rush is aiming to serve as a multiple usage service of BMS ecosystem.

    <n-list>
      <n-list-item>
        It's a BMS package download website
      </n-list-item>
    </n-list>
    TBD
  </n-card>
  <n-card title="Request to upload a package">
    <n-p>
    If you found you have a bms package that doesn't present on gingerrush, you can request to upload them.
    First, you'll have to compress your bms files into a simple compressed archive.
    Suppose you want to upload this package(硬核機動), it's a directory stored on your disk: 
    </n-p>
    <pre>
      硬核機動 
      ├── Kokaku_whiteout.bmp
      ├── oppo7.bms 
      ├── preview_music.ogg
      ├── 新しいテキスト ドキュメント.txt
      ├── 硬核機動_DPA_.bms
      ├── 硬核機動_DPA.bms
      ├── 硬核機動_DPH.bms
      ├── 硬核機動_DPN.bms
      ├── 硬核機動[5K].bms
      ├── 硬核機動[9KEYS-H].pms
      └── .......
    </pre>
    <n-p>
      You need to compress the whole directory (硬核機動) into a single archive file, and put it on any file share service. I suggest using <a href="https://gofile.io">gofile</a>.
      After packaging it, you'll need to open a ticket on github, or more accurately, open a new issue in this github repo: <a href="https://github.com/Catizard/Ginger-Tracker">Ginger-Tracker</a> and everything will be handled then.
    </n-p>
  </n-card>
  <n-card title="Contributors">
    <n-list>
      <n-list-item>
        <n-flex>
          Catizard
          <n-button text size="small">
            <n-icon>
              <GithubIcon />
            </n-icon>
          </n-button>
        </n-flex>
      </n-list-item>
    </n-list>
  </n-card>
</template>

<script setup lang="ts">
import { LogoGithub as GithubIcon } from '@vicons/ionicons5'
</script>
