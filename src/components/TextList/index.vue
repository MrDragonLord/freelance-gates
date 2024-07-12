<template>
  <div :class="bemBlock" :data-alignment="alignment" ref="container">
    <div
      v-for="(tag, index) in visibleTags"
      :key="index"
      :ref="`tag-${index}`"
      :class="bemElement('tag-wrapper')"
    >
      <span :class="bemElement('tag')">{{ tag.text }}</span>
      <v-icon v-if="tag.icon">{{ tag.icon }}</v-icon>
      <v-icon v-if="index < visibleTags.length - 1" class="separator">mdi-circle-small</v-icon>
    </div>
  </div>
</template>

<script>
import { VIcon } from "vuetify/lib/components";

export default {
  name: 'TextList',
  components: { VIcon },
  props: {
    tags: {
      type: Array,
      required: true
    },
    alignment: {
      type: String,
      default: 'left'
    }
  },
  data() {
    return {
      bemBlock: 'text-tags',
      visibleTags: [...this.tags],
    };
  },
  computed: {
    bemElement() {
      return (element) => `${this.bemBlock}__${element}`;
    }
  },
  mounted() {
    this.calculateVisibleTags();
    window.addEventListener('resize', this.calculateVisibleTags);
  },
  beforeDestroy() {
    window.removeEventListener('resize', this.calculateVisibleTags);
  },
  methods: {
    calculateVisibleTags() {
      this.visibleTags = [...this.tags]
      this.$nextTick(() => {
        const containerWidth = this.$refs.container.offsetWidth;

        let totalWidth = 0;
        this.visibleTags = [];

        for (let index = 0; index < this.tags.length; index++) {
          const tagElement = this.$refs[`tag-${index}`][0];
          if (!tagElement) {
            continue;
          }

          const tagWidth = tagElement.offsetWidth;

          if (totalWidth + tagWidth > containerWidth) {
            break;
          }

          this.visibleTags.push(this.tags[index]);
          totalWidth += tagWidth;
        }
      });
    }
  }
};
</script>

<style lang="scss">
.text-tags {
  display: flex;
  align-items: center;
  flex-wrap: wrap;

  &__tag-wrapper {
    display: flex;
    align-items: center;

    &:not(:last-child) .separator {
      margin: 0 4px;
    }
  }

  &__tag {
    white-space: nowrap;
  }

  &[data-alignment='left'] {
    justify-content: flex-start;
  }

  &[data-alignment='justify'] {
    justify-content: space-between;
  }
}
</style>
