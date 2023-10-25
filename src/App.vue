<template>
  <div class="list-container">
    <div class="item-container" v-for="(item, index) in svgList" :key="index">
      <img class="item-svg" v-lazy="item.path" />
      <div id="loading-overlay"><div class="loading-spinner"></div></div>
      <p class="item-title">{{ item.title }}</p>
    </div>
  </div>
</template>

<script>
export default {
  name: "App",
  data() {
    return {
      svgList: [],
    };
  },
  mounted() {
    const req = require.context("./assets", false, /\.svg$/);
    const requireAll = (requireContext) => requireContext.keys();
    const re = /\.\/(.*)\.svg/;
    this.svgList = requireAll(req).map((i) => {
      return {
        path: require("./assets/" + i.replace("./", "")),
        title: i.match(re)[1],
      };
    });
  },
};
</script>

<style lang="scss">
html,
body {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  margin: 0;
  padding: 0;
  font-size: 16px;
  background-color: #ffffff;

  .list-container {
    padding: 1.25rem;
    display: grid;
    gap: 1.25rem;

    .item-container,
    .item-svg,
    #loading-overlay {
      width: 100%;
      height: auto;
      min-width: 1.25rem;
    }

    .item-container {
      position: relative;

      .item-title {
        text-align: center;
        margin: 1rem 0 0;
        word-break: break-all;
        font-size: 0.75rem;
      }

      .item-svg,
      #loading-overlay {
        aspect-ratio: 19/12;
      }

      #loading-overlay {
        position: absolute;
        top: 0;
        left: 0;
        background-color: rgba(255, 255, 255, 0.8);
        outline: 1px dashed #ccc;
        border-radius: 4px;
        overflow: hidden;
        display: flex;
        justify-content: center;
        align-items: center;
        opacity: 0;
        pointer-events: none;
        transition: all 0.3s ease-in-out;
      }

      @keyframes spin {
        0% {
          transform: rotate(0deg);
        }

        100% {
          transform: rotate(360deg);
        }
      }

      img[lazy="loading"] + #loading-overlay {
        opacity: 1;
        pointer-events: all;
        .loading-spinner {
          border: 0.1875rem solid #f3f3f3;
          border-top: 0.1875rem solid #3498db;
          border-radius: 50%;
          width: 1.25rem;
          height: 1.25rem;
          animation: spin 2s linear infinite;
        }
      }
    }

    @media (min-width: 1280px) {
      grid-template-columns: repeat(15, 1fr);
    }

    @media (min-width: 992px) and (max-width: 1279px) {
      grid-template-columns: repeat(12, 1fr);
    }

    @media (min-width: 768px) and (max-width: 992px) {
      grid-template-columns: repeat(9, 1fr);
    }

    @media (min-width: 576px) and (max-width: 767px) {
      grid-template-columns: repeat(6, 1fr);
    }

    @media (max-width: 575px) {
      grid-template-columns: repeat(3, 1fr);
    }
  }
}
</style>
