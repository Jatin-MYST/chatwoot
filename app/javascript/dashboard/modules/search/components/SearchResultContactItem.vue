<template>
  <router-link :to="navigateTo" class="contact-item">
    <woot-thumbnail :src="thumbnail" :username="name" size="24px" />
    <div class="contact-details">
      <h5 class="text-block-title name">{{ name }}</h5>
      <p class="details-meta">
        <span v-if="email" class="email">{{ email }}</span>
        <span v-if="phone" class="separator">•</span>
        <span v-if="phone" class="phone">
          {{ phone }}
        </span>
      </p>
    </div>
  </router-link>
</template>

<script>
import { frontendURL } from 'dashboard/helper/URLHelper';
export default {
  props: {
    id: {
      type: [String, Number],
      default: 0,
    },
    email: {
      type: String,
      default: '',
    },
    phone: {
      type: String,
      default: '',
    },
    name: {
      type: String,
      default: '',
    },
    thumbnail: {
      type: String,
      default: '',
    },
    accountId: {
      type: [String, Number],
      default: 0,
    },
  },
  computed: {
    navigateTo() {
      return frontendURL(`accounts/${this.accountId}/contacts/${this.id}`);
    },
  },
};
</script>

<style scoped lang="scss">
.contact-item {
  @apply cursor-pointer flex items-center p-2 rounded-sm hover:bg-slate-25 dark:hover:bg-slate-800;
}
.contact-details {
  @apply ml-2 rtl:mr-2 rtl:ml-0;
}
.name {
  @apply m-0;
}
.details-meta {
  @apply m-0 text-slate-600 dark:text-slate-200 text-sm flex items-center;

  span {
    @apply ml-1 rtl:mr-1 rtl:ml-0;
  }
}
</style>
