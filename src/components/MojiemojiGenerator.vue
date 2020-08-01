<template>
  <div class="mojiemoji-generator">
    <div class="preview">
      <div class="frame" :data-text-length="splitedText.length" ref="frame">
        <div class="inner" :style="style">
          <span v-for="(char, i) in splitedText" :key="i">{{ char }}</span>
        </div>
      </div>
    </div>
    <input v-model="style.color" />
    <input v-model="style.fontFamily" />
    <input v-model="style.fontWeight" type="number" min="100" max="900" step="100" />
    <input v-model="text" />
    <button @click="generateImage">生成</button>
    <section class="download" v-if="image">
      <a :href="image" :download="`${text}.png`" class="image">
        <img :src="image" />
      </a>
      <p :style="{ 'margin-top': '16px' }">画像をクリックでダウンロード</p>
    </section>
  </div>
</template>

<script>
import html2canvas from 'html2canvas'

export default {
  name: 'MojiemojiGenerator',
  data () {
    return {
      style: {
        color: '#2c3e50',
        fontFamily: 'sans-serif',
        fontWeight: '900',
      },
      text: 'あいうえ',
      image: null,
    };
  },
  computed: {
    splitedText () {
      return [...this.text]
    },
  },
  methods: {
    async generateImage () {
      const canvas = await html2canvas(this.$refs.frame, {
        backgroundColor: null,
      })
      this.image = canvas.toDataURL('image/png')
    },
  },
};
</script>

<style lang="scss" scoped>
section {
  margin: 40px;
}

.preview {
  width: 320px;
  height: 320px;
  margin: 40px auto;
  box-shadow: 0 2px 4px 0 rgba(0,0,0,0.2);
}

.frame {
  width: 320px;
  height: 320px;
  display: flex;
  align-items: center;
  justify-content: center;
  
  line-height: 1;

  .inner {
    display: flex;
    flex-wrap: wrap;
  }

  span {
    text-align: center;
  }
}
.frame[data-text-length='1'] {
  font-size: 320px;

  span {
    width: 320px;
    height: 320px;
  }
}
.frame[data-text-length='2'],
.frame[data-text-length='3'],
.frame[data-text-length='4'] {
  font-size: 160px;

  span {
    width: 160px;
    height: 160px;
  }
}
.frame[data-text-length='5'],
.frame[data-text-length='6'],
.frame[data-text-length='7'],
.frame[data-text-length='8'],
.frame[data-text-length='9'] {
  font-size: 106.66px;

  span {
    width: 106.66px;
    height: 106.66px;
  }
}

.image {
  display: block;
  width: 320px;
  height: 320px;
  margin: 0 auto;
  box-shadow: 0 2px 4px 0 rgba(0,0,0,0.2);
}
</style>