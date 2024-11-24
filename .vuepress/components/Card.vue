<template>
  <div class="user-grid">
    <div v-for="user in users" :key="user.uid" :class="getCardClass(user)">
      <a :href="user.url" target="_blank" rel="noopener noreferrer">
        <img
          :src="`https://a.ppy.sh/${user.uid}`"
          :alt="user.name"
          class="user-avatar"
          @error="handleImageError"
        />
        <div class="user-info">
          <span class="user-name">{{ user.name }}</span>
          <slot name="extra" :user="user"></slot>
        </div>
      </a>
    </div>
  </div>
</template>

<script setup lang="ts">
interface User {
  disable?: boolean;
  uid: string;
  name: string;
  url: string;
  date?: number[];
}

defineProps<{
  users: User[];
}>();

const handleImageError = (e: Event) => {
  const img = e.target as HTMLImageElement;
  img.src = "../public/images/defaultAvatar.png";
};

const getCardClass = (user) => {
  if (user.disable) {
    return "user-card disable";
  } else if (user.stuff) {
    return "user-card stuff";
  } else {
    return "user-card";
  }
};
</script>

<style scoped lang="scss">
.user-grid {
  display: grid;
  grid-template-columns: repeat(auto-fill, minmax(200px, 1fr));
  gap: 1rem;
  padding: 1rem;
}

.user-card {
  display: flex;
  align-items: center;
  padding: 0.5rem;
  border-radius: 0.5rem;
  background: var(--custom-bg-lighter);
  transition: transform 0.2s;
}

.disable {
  &:hover {
    transform: none !important;
  }
  &::before {
    content: "";
    position: absolute;
    top: 0;
    left: 0;
    width: 100%;
    height: 100%;
    z-index: 1;
    pointer-events: all;
    border-radius: 0.5rem;
    background-color: rgba(0, 0, 0, 0.3);
    cursor: not-allowed;
  }

  filter: grayscale(100%);
}

.stuff {
  position: relative;
  box-shadow: 0 4px 12px rgba(0, 0, 0, 0.1);

  .user-name {
    background: linear-gradient(
      45deg,
      var(--custom-primary-color, #4ecdc4),
      var(--custom-secondary-color, #ff6b6b)
    );
    -webkit-background-clip: text;
    background-clip: text;
    color: transparent;
    font-size: 1.1em;
  }

  &::after {
    content: "";
    position: absolute;
    top: 0;
    left: 0;
    width: 100%;
    height: 100%;
    z-index: -1;
    border-radius: 0.5rem;
    opacity: 0;
    background: var(--custom-hover-bg, rgba(255, 255, 255, 0.1));
    transition: opacity 0.3s ease;
  }

  &:hover::after {
    opacity: 1;
  }
}

.user-card:hover {
  transform: translateY(-2px) scale(1.02);
}

.user-card a {
  display: flex;
  align-items: center;
  text-decoration: none;
  color: var(--custom-text);
  width: 100%;
}

.user-avatar {
  width: 50px;
  height: 50px;
  border-radius: 50%;
  object-fit: cover;
  margin-right: 1rem;
}

.user-info {
  display: flex;
  flex-direction: column;
}

.user-name {
  font-weight: bold;
  margin-bottom: 0.25rem;
}
</style>
