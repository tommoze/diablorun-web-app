<template>
  <v-card elevation="1" class="fill-height pa-2">
    <v-row no-gutters>
      <v-col cols="auto">
        <v-img :src="imageSrc" contain />
      </v-col>
      <v-col align="center">
        <h5>
          <span v-if="runeword" class="quality-gold">
            {{ runeword }}<br />
          </span>
          <span :class="`quality-${runeword ? 'socketed' : item.quality}`">
            {{ runeword ? item.base_name : item.name }}
          </span>
        </h5>
        <p
          class="mb-0 body-2"
          :class="{ 'error--text': property.includes('ÿc1') }"
          v-for="property of properties"
          :key="property"
        >
          {{ property.replace(/^ÿc1/, '') }}
        </p>
      </v-col>
    </v-row>
  </v-card>
</template>

<style scoped lang="scss">
.quality-yellow {
  color: #fdfd70;
}

.quality-orange {
  color: #f7c100;
}

.quality-green {
  color: #01fd00;
}

.quality-gold {
  color: #dcc784;
}

.quality-blue {
  color: #7575fd;
}

.quality-socketed {
  opacity: 0.5;
  font-weight: 400;
}

.quality-white {
  color: #fdfdfd;
}
</style>

<script>
import { itemImages } from '@diablorun/diablorun-data';

export default {
  name: 'CharacterItem',
  props: {
    item: Object
  },
  data() {
    return {
      imageSrc: '',
      properties: [],
      runeword: ''
    };
  },
  watch: {
    item: {
      immediate: true,
      handler(item) {
        this.imageSrc = itemImages[item.name] || itemImages[item.base_name];
        this.properties = JSON.parse(item.properties);

        const runewordMatch = item.name.match(/^(.*?) \[(.*?)\]$/);

        if (runewordMatch) {
          this.runeword = runewordMatch[2];
        } else {
          this.runeword = '';
        }
      }
    }
  }
};
</script>
