<template>
    <div class="card card-copy">
        <div class="card-block m-2">
            @{{ year }}
            <a href="https://medcenter24.com" target="_blank">MedCenter24.com</a>
          <!-- todo
            <router-link to="/about">{{ $t('About') }}</router-link>
            <router-link to="/help">{{ $t('Help') }}</router-link>
            <router-link to="/privacy">{{ $t('Privacy') }}</router-link>
            <router-link to="/terms">{{ $t('Terms') }}</router-link>
            -->
        </div>
    </div>
</template>
<style lang="scss">
    @import "../../sass/variables";
    .card-copy {
        font-size: $font-size-sm;
    }
</style>
<script>
  export default {
    components: {},
    data () {
      return {
        year: (new Date()).getFullYear()
      }
    }
  }
</script>
