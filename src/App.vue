<template>
  <div id="app">
    <notifications group="foo" />
    <router-view />
  </div>
</template>

<script>
import loading from './cmps/loading.cmp.vue';
export default {
  components: {
    loading,
  },

  data() {
    return {
      isLoading: false,
    };
  },
  created() {
    this.$isLoading(true);
    setTimeout(() => this.$isLoading(false), 1500);
    this.$store.dispatch({ type: 'loadTemplates' });

    if (window.innerWidth < 500) {
      this.$store.dispatch({ type: 'isMobile', isMobile: true });
    } else {
      this.$store.dispatch({ type: 'isMobile', isMobile: false });
    }

    this.$nextTick(() => {
      window.addEventListener('resize', this.onResize);
    });
  },
  beforeDestroy() {
    window.removeEventListener('resize', this.onResize);
  },
  methods: {
    onResize() {
      this.mediaWidth = window.innerWidth;
      this.onResizeScreen();
    },
    onResizeScreen() {
      this.mediaWidth = window.innerWidth;
      clearTimeout(this.debounceScreen);
      this.debounceScreen = setTimeout(() => {
        if (this.mediaWidth < 500) {
          // this.$isLoading(true)
          // setTimeout(() => this.$isLoading(false), 1000)
          this.$store.dispatch({ type: 'isMobile', isMobile: true });
        } else {
          this.$isLoading(true);
          setTimeout(() => this.$isLoading(false), 1000);
          this.$store.dispatch({ type: 'isMobile', isMobile: false });
        }
      }, 200);
    },

    setDeviceType(ev) {},
  },
};
</script>
