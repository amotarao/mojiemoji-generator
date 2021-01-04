<template>
  <div class="mojiemoji-generator">
    <section>
      <h1>mojiemoji-generator</h1>
      <GithubButton
        href="https://github.com/amotarao/mojiemoji-generator"
        data-size="large"
        aria-label="Star amotarao/mojiemoji-generator on GitHub"
        >Star</GithubButton
      >
    </section>

    <section class="preview">
      <div class="frame" :data-text-length="splitedText.length" ref="frame">
        <div class="inner" :style="style">
          <span v-for="(char, i) in splitedText" :key="i">{{ char }}</span>
        </div>
      </div>
    </section>

    <section>
      <form @submit.prevent>
        <input v-model="style.color" type="color" />
        <input v-model="style.fontFamily" />
        <input v-model="style.fontWeight" type="number" min="100" max="900" step="100" />
        <input v-model="text" />
        <button type="submit" @click="generateImage">生成</button>
      </form>
    </section>

    <section class="download" v-if="image">
      <a :href="image" :download="`${text}.png`" class="image">
        <img :src="image" />
      </a>
      <p>画像をクリックでダウンロード</p>
    </section>
  </div>
</template>

<script>
import html2canvas from 'html2canvas';
import GithubButton from 'vue-github-button';

export default {
  name: 'MojiemojiGenerator',
  components: {
    GithubButton,
  },
  data() {
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
    splitedText() {
      return [...this.text];
    },
  },
  methods: {
    async generateImage() {
      const canvas = await html2canvas(this.$refs.frame, {
        backgroundColor: null,
      });
      this.image = canvas.toDataURL('image/png');
    },
  },
};
</script>

<style lang="scss" scoped>
section {
  margin: 40px;
}

h1 {
  margin-bottom: 24px;
}

.preview {
  width: 320px;
  height: 320px;
  margin: 40px auto;
  box-shadow: 0 2px 4px 0 rgba(0, 0, 0, 0.2);
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
  box-shadow: 0 2px 4px 0 rgba(0, 0, 0, 0.2);

  img {
    height: auto;
    max-height: 100%;
    max-width: 100%;
    width: auto;
  }
}

.download {
  p {
    margin-top: 16px;
  }
}
</style>
