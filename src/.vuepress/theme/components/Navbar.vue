<template>
  <header
    :class="
      classNames(
        'fixed top-0 left-0 z-[50] bg-levelOne/90 border-b-none flex flex-row justify-between py-2 px-4 md:px-10 lg:px-14 h-14',
        'w-[calc(100vw-32px)] md:w-[calc(100vw-80px)] lg:w-[calc(100vw-112px)]'
      )
    "
  >
    <SidebarButton @toggle-sidebar="$emit('toggle-sidebar')" />

    <RouterLink :to="$localePath" class="ml-12 h-full md:ml-0">
      <img
        v-if="$site.themeConfig.logo"
        class="h-full"
        src="/assets/stratum-v2-icon-with-text.svg"
        :alt="$siteTitle"
      />
    </RouterLink>

    <div class="hidden lg:block">
      <div class="flex space-x-16">
        <NavLink
          v-for="item of nav"
          :key="item.link"
          :item="item"
          :class="
            classNames(
              'hover:text-links text-sm lg:text-lg',
              isActive($route, item.link) && 'text-links'
            )
          "
        />
      </div>
    </div>
  </header>
</template>

<script>
import SearchBox from "@SearchBox";
import SidebarButton from "@theme/components/SidebarButton.vue";
import NavLink from "@theme/components/NavLink.vue";
import { classNames, isActive } from "../../utils";

export default {
  name: "Navbar",
  components: {
    SidebarButton,
    NavLink,
    SearchBox,
  },
  data() {
    return {
      linksWrapMaxWidth: null,
    };
  },
  computed: {
    nav() {
      return this.$themeLocaleConfig.nav || this.$site.themeConfig.nav || [];
    },
  },
  methods: { classNames, isActive },
  mounted() {
    const MOBILE_DESKTOP_BREAKPOINT = 719; // refer to config.styl
    const NAVBAR_VERTICAL_PADDING =
      parseInt(css(this.$el, "paddingLeft")) +
      parseInt(css(this.$el, "paddingRight"));
    const handleLinksWrapWidth = () => {
      if (document.documentElement.clientWidth < MOBILE_DESKTOP_BREAKPOINT) {
        this.linksWrapMaxWidth = null;
      } else {
        this.linksWrapMaxWidth =
          this.$el.offsetWidth -
          NAVBAR_VERTICAL_PADDING -
          ((this.$refs.siteName && this.$refs.siteName.offsetWidth) || 0);
      }
    };
    handleLinksWrapWidth();
    window.addEventListener("resize", handleLinksWrapWidth, false);
  },
};
function css(el, property) {
  // NOTE: Known bug, will return 'auto' if style value is 'auto'
  const win = el.ownerDocument.defaultView;
  // null means not to return pseudo styles
  return win.getComputedStyle(el, null)[property];
}
</script>

<style lang="stylus">
@import "../../styles/theme.styl";
</style>
