<template>
  <div class="main" @dragenter.prevent @dragover.prevent @drop.prevent="onDrop">
    <progress
      v-if="uploadProgress !== null"
      :value="uploadProgress"
      max="100"
      class="progress-bar"
    ></progress>
    <UploadPopup
      v-model="showUploadPopup"
      @upload="onUploadClicked"
      @createFolder="createFolder"
    ></UploadPopup>
    <button class="upload-button circle" @click="showUploadPopup = true">
      <img
        class="icon"
        src="https://cdnjs.cloudflare.com/ajax/libs/material-design-icons/4.0.0/png/file/upload_file/materialicons/36dp/2x/baseline_upload_file_black_36dp.png"
        alt="Upload"
        width="36"
        height="36"
        @contextmenu.prevent
      />
    </button>
    <div class="app-bar">
      <input
        type="search"
        v-model="search"
        class="search-bar"
        placeholder="Search"
        aria-label="Search"
      />
      <div class="menu-button">
        <button class="circle" @click="showMenu = true">
          <svg
            xmlns="http://www.w3.org/2000/svg"
            viewBox="0 0 448 512"
            width="24"
            height="24"
            class="menu-icon"
          >
            <path
              d="M120 256c0 30.9-25.1 56-56 56s-56-25.1-56-56s25.1-56 56-56s56 25.1 56 56zm160 0c0 30.9-25.1 56-56 56s-56-25.1-56-56s25.1-56 56-56s56 25.1 56 56zm104 56c-30.9 0-56-25.1-56-56s25.1-56 56-56s56 25.1 56 56s-25.1 56-56 56z"
            />
          </svg>
        </button>
        <Menu
          v-model="showMenu"
          :items="[{ text: '名称A-Z' }, { text: '大小↑' } ,{ text: '大小↓' }, { text: '粘贴' }]"
          @click="onMenuClick"
        />
      </div>
    </div>
    <ul class="file-list">
      <!-- 文件列表内容 -->
    </ul>
    <div v-if="loading" class="loading-message">
      <span>加载中...</span>
    </div>
    <div
      v-else-if="!filteredFiles.length && !filteredFolders.length"
      class="empty-message"
    >
      <span>没有文件</span>
    </div>
    <Dialog v-model="showContextMenu">
      <div
        v-text="focusedItem.key || focusedItem"
        class="contextmenu-filename"
        @click.stop.prevent
      ></div>
      <!-- 上下文菜单内容 -->
    </Dialog>
  </div>
</template>

<script>
/* Vue逻辑与前面一致 */
</script>

<style>
/* 主容器样式 */
.main {
  height: 100%;
  font-family: -apple-system, BlinkMacSystemFont, "Helvetica Neue", Arial, sans-serif;
  background-color: #f8f9fa;
}

/* App Bar 样式 */
.app-bar {
  position: sticky;
  top: 0;
  padding: 8px;
  background-color: white;
  box-shadow: 0 2px 3px rgba(0, 0, 0, 0.1);
  display: flex;
  align-items: center;
}

/* 搜索框样式 */
.search-bar {
  flex: 1;
  padding: 8px 12px;
  font-size: 14px;
  border: 1px solid #e5e5ea;
  border-radius: 8px;
  background-color: #f2f2f7;
}

/* 上传按钮样式 */
.upload-button {
  position: fixed;
  bottom: 16px;
  right: 16px;
  width: 56px;
  height: 56px;
  background: linear-gradient(135deg, #0a84ff, #5ac8fa);
  border: none;
  border-radius: 50%;
  box-shadow: 0 4px 6px rgba(0, 0, 0, 0.2);
  display: flex;
  align-items: center;
  justify-content: center;
  cursor: pointer;
}

/* 图标样式 */
.icon {
  filter: invert(100%);
}

/* 文件列表样式 */
.file-list {
  list-style: none;
  padding: 0;
  margin: 0;
}

.file-item {
  display: flex;
  align-items: center;
  padding: 12px;
  border-bottom: 1px solid #e5e5ea;
  transition: background-color 0.2s ease;
}

.file-item:hover {
  background-color: #f2f2f7;
}

/* 其他必要样式省略 */
</style>

