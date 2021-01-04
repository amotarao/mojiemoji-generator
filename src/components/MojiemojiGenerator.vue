<template>
  <div :class="$style.wrapper">
    <section :class="$style.section">
      <h1 :class="$style.title">mojiemoji-generator</h1>
      <a
        class="github-button"
        href="https://github.com/amotarao/mojiemoji-generator"
        data-color-scheme="no-preference: light; light: light; dark: dark;"
        data-size="large"
        aria-label="Star amotarao/mojiemoji-generator on GitHub"
        >Star</a
      >
    </section>

    <section :class="$style.preview">
      <div :class="$style.frame" :data-text-length="splitedText.length" ref="frameRef">
        <div :class="$style.inner" :style="style">
          <span v-for="(char, i) in splitedText" :key="i">{{ char }}</span>
        </div>
      </div>
    </section>

    <section :class="$style.section">
      <form @submit.prevent>
        <input v-model="style.color" type="color" />
        <input v-model="style.fontFamily" />
        <input v-model="style.fontWeight" type="number" min="100" max="900" step="100" />
        <input v-model="text" />
        <button type="submit" @click="generate">生成</button>
      </form>
    </section>

    <section :class="$style.download" v-if="image">
      <a :href="image" :download="`${text}.png`" :class="$style.image">
        <img :src="image" :alt="`${text}`" />
      </a>
      <p>画像をクリックでダウンロード</p>
    </section>
  </div>
</template>

<script>
import { computed, defineComponent, reactive, ref, toRefs } from 'vue';
import html2canvas from 'html2canvas';

export default defineComponent({
  name: 'MojiemojiGenerator',
  setup() {
    const state = reactive({
      style: {
        color: '#2c3e50',
        fontFamily: 'sans-serif',
        fontWeight: '900',
      },
      text: 'あいうえ',
      image: null,
    });

    const frameRef = ref();

    const splitedText = computed(() => {
      return [...state.text];
    });

    const generate = async () => {
      console.log(frameRef.value);
      const canvas = await html2canvas(frameRef.value, {
        backgroundColor: null,
      });
      state.image = canvas.toDataURL('image/png');
    };

    return {
      ...toRefs(state),
      frameRef,
      splitedText,
      generate,
    };
  },
});
</script>

<style lang="scss" module>
.section {
  margin: 40px;
}

.title {
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
